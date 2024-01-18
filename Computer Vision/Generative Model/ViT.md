# Vision-Transformer in ViTDet/SAM

以 SAM 的 ViT 为例，在 SAM 中，输入图像的大小为 [1024, 1024, 3]。

|           Variables            |      `ViT-H`       |      `ViT-L`       |      `ViT-B`       |
| :----------------------------: | :----------------: | :----------------: | :----------------: |
|            `depth`             |         32         |         24         |         12         |
|          `embed_dim`           |        1280        |        1024        |        768         |
|          `num_heads`           |         16         |         16         |         12         |
|     `global_attn_indexes`      |  [7, 15, 23, 31]   |  [5, 11, 17, 23]   |   [2, 5, 8, 11]    |
| Estimated Number of Parameters |    635,905,280     |    307,291,136     |     88,790,784     |
|   Estimated Number of FLOPs    | 94,903,292,859,392 | 47,409,932,196,864 | 15,301,029,795,840 |

### 参数量估计

1. 输入编码：
   1. Patch Embedding Weight：3 * E * 16 ^ 2 = 768 * E
   2. Patch Embedding Bias: E
   3. Absolute Positional Embedding：64 * 64 * E = 4096 * E
2. Block：
   1. Self-Attention：4 * E ^ 2 + 4 * E
   2. MLP：8 * E ^ 2 + 5 * E
   3. LayerNorm：2 * 2 * E
   4. Relative Positional Embbeding：较小的常数
   5. 总计：12 * E ^ 2 + 13 * E

总计：(12 * E ^ 2 + 13 * E) * D + 4865 * E。

### 计算量估计

1. Attention：这里假设矩阵乘法用的是 $O(N^3)$ 的算法，注意 B, H, W 会变化：
   1. 输入到 QKV：2 * B * N * H * W * E ^ 2 * 3
   2. QK：2 * (B * N * H * W) ^ 2 * (E / B / N)
   3. Relative Positional Embedding：2 * B * N * (H * W) * (E / B / N)
   4. Softmax：3 * B * N * (H * W) ^ 2
   5. QKV：2 * B * N * (H * W) ^ 2 * (E / B / N)
   6. Multi-Head：2 * B * N * H * W * E ^ 2
2. MLP：
   1. 矩阵：2 * N * H * W * E ^ 2 * 8
   2. 激活函数：5 * N * H * W * E


总计：4 * (B * N * H * W) * (8 * E ^ 2 + 4 * H * W * E + 3 * H * W + 2 * E / B / N) + (D - 4) * (B' * N * H' * W') * (8 * E ^ 2 + 4 * H' * W' * E + 3 * H' * W' + 2 * E / B / N) + D * (N * H * W * E) * (16 * E + 5)。

B, H, W, B', H', W' = 1, 64, 64, 25, 14, 14

`E` 代表 `embed_dim`，`N` 代表 `num_heads`。

## Modules

### `PatchEmbed`

`PatchEmbed` 的作用是将输入的图像转换为 Transformer 可以接受的 token 形式，即 $16\times16$ 的图像 patch 为一个 token。

```python
patch_embed = nn.Conv2d(
    in_channels = 3,          # 3 channels image
    out_channels = embed_dim, # `embed_dim` channels feature
    kernel_size = (16, 16),
    stride = (16, 16),
    padding = 0
)
```

|     Meanings      |        Universal         |       ViT-H        |
| :---------------: | :----------------------: | :----------------: |
|       Input       |      `[B, C, H, W]`      | [1, 3, 1024, 1024] |
| After Convlution  | `[B, E, H / 16, W / 16]` | [1, 1280, 64, 64]  |
| After Permutation | `[B, H / 16, W / 16, E]` | [1, 64, 64, 1280]  |

### `PosEmbed`

`PosEmbed` 的作用是给输入的 token 加上绝对位置编码，其大小和 token 一样。

```python
pos_embed = nn.Parameter(torch.zeros(1, H / 16, W / 16, embed_dim))
```

### `Block`

`Block` 是 Transformer 的 Encoder Block。

```python
class Block(nn.Module):
    def __init__(self) -> None:
        ... # LayerNorm, Attention, Drop Path ...
        self.mlp = Mlp(
            in_features = embed_dim,
            hidden_features = embed_dim * 4,
            act_layer = nn.GELU
        )

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        x = x + self.attn(self.norm1(x))
        x = x + self.mlp(self.norm2(x))
        return x
```

|    Meanings     |   Universal    |       ViT-H       |
| :-------------: | :------------: | :---------------: |
|      Input      | `[B, H, W, E]` | [1, 64, 64, 1280] |
| First Residual  | `[B, H, W, E]` | [1, 64, 64, 1280] |
| Second Residual | `[B, H, W, E]` | [1, 64, 64, 1280] |

### `Attention`

`Attention` 是 Transformer 的 Attention 机制。其中引入了 [Window Attention](https://arxiv.org/abs/2103.14030) + 相对位置编码，在某几个 Block 内使用的是[全局 Attention](https://arxiv.org/abs/2203.16527) + 相对位置编码。

```python
class Attention(nn.Module):
    def __init__(self, ...) -> None:
        self.qkv = nn.Linear(
        	in_features = embed_dim,
            out_features = embed_dim * 3,
            bias = True
        )
        self.proj = nn.Linear(
        	in_features = embed_dim,
            out_features = embed_dim
        )
        self.rel_pos_h = nn.Parameter(torch.zeros(2 * input_size[0] - 1, head_dim))
        self.rel_pos_w = nn.Parameter(torch.zeros(2 * input_size[0] - 1, head_dim))

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        q, k, v = self.qkv(x).unbind(0)

        qk = (q * self.scale) @ K.transpose(-2, -1)
        qk_embed = add_decomposed_rel_pos(qk, q, self.rel_pos_h, self.rel_pos_w, (H, W), (H, W))
        qk_softmax = qk_embed.softmax(dim = -1)
        attn = (qk_softmax @ v)

        output = self.proj(attn)
        return output
```

#### Relative Postional Embedding

[该文章](https://arxiv.org/pdf/1803.02155.pdf) 认为，自注意力机制很有可能丢失相对位置信息，因此在自注意力内机制引入了相对位置编码。这里采用的是 [Decomposed Relative Postional Embeding](https://arxiv.org/abs/2112.01526) 中的方法，即将相对位置编码分解为两个维度，分别对应行和列，相应的 Attention 为

$$
\begin{align*}
\mathrm{Attn}(Q,K,V)&=\mathrm{Softmax}(QK^T/\sqrt{d}+Q\odot R_{p(i),p(j)})V\\
R_{p(i),p(j)}&=R_{h(i),h(j)}+R_{w(i),w(j)}
\end{align*}
$$

#### Attention

|        Meanings         |         Universal         |        ViT-H         |
| :---------------------: | :-----------------------: | :------------------: |
|          Input          |      `[B, H, W, E]`       |  [1, 64, 64, 1280]   |
|           QKV           | `[3, B, N, H * W, E / N]` | [3, 1, 16, 4096, 80] |
|          Q/K/V          |  `[B * N, H * W, E / N]`  |    [16, 4096, 80]    |
|           QK            |  `[B * N, H * W, H * W]`  |   [16, 4096, 4096]   |
| After Relative PosEmbed |  `[B * N, H * W, H * W]`  |   [16, 4096, 4096]   |
|        Attention        |      `[B, H, W, E]`       |  [1, 64, 64, 1280]   |
|         Output          |      `[B, H, W, E]`       |  [1, 64, 64, 1280]   |

#### Window Attention

Window Attention 的作用是减少了 Attention 的计算量。

```python
# In Block.forward(), after first LayerNorm
...
x, pad_hw = window_partition(x, self.window_size)
x = self.attn(x)
x = window_unpartition(x, self.window_size, pad_hw, (H, W))
...
```

|        Meanings         |         Universal         |        ViT-H         |
| :---------------------: | :-----------------------: | :------------------: |
|          Input          |      `[B, H, W, E]`       |  [25, 14, 14, 1280]  |
|           QKV           | `[3, B, N, H * W, E / N]` | [3, 25, 16, 196, 80] |
|          Q/K/V          |  `[B * N, H * W, E / N]`  |    [400, 196, 80]    |
|           QK            |  `[B * N, H * W, H * W]`  |   [400, 196, 196]    |
| After Relative PosEmbed |  `[B * N, H * W, H * W]`  |   [400, 196, 196]    |
|        Attention        |      `[B, H, W, E]`       |  [25, 14, 14, 1280]  |
|         Output          |      `[B, H, W, E]`       |  [25, 14, 14, 1280]  |

## Others

所有的 Positional Embedding 都用 `nn.init.trunc_normal_(pos_embed, std=0.02)` 初始化。

所有的 `nn.Linear` 的权重都用 `nn.init.trunc_normal_(weight, std=0.02)` 初始化。

所有的 `nn.Linear` 的偏置都用 `nn.init.constant_(bias, 0)` 初始化。

所有的 `nn.LayerNorm` 的都用 `nn.init.constant_(bias, 0)` + `nn.init.constant_(weight, 0)` 初始化。
