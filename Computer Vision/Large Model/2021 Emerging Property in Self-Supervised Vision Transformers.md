# Emerging Property in Self-Supervised Vision Transformers

## 1 Introduction

文章主要讨论了 ViT 自监督的能力：
- ViT 自监督的特征图可以包含场景的布局，特别是物体的轮廓；
- ViT 自监督可以直接用于 KNN 分类，可以在分类任务上调优。

DINO (Distillation with No Labels) 的学生网络使用 Cross Entropy Loss，预测教师网络的输出。教师网络的参数更新使用 Exponential Moving Average (EMA)，EMA 的参数是从 0.996 到 1 余弦变化。

## 2 Related Work

知识蒸馏一般固定教师网络，但是在 DINO 中，教师网络是动态更新的，二者的网络结构也是相同的。

## 3 Approach

|            Symbols             |       Descriptions       |
| :----------------------------: | :----------------------: |
|              $x$               |       input image        |
| $g_{\theta_s}$, $g_{\theta_t}$ | student, teacher network |
|          $P_s$, $P_t$          |      $K$ dim output      |
|       $\tau_s$, $\tau_t$       |       temperature        |

```python
def train_step():
    for x in loader:
        x1, x2 = augment(x), augment(x) # [N, ...]
        s1, s2 = gs(x1), gs(x2) # [N, K]
        t1, t2 = gt(x1), gt(x2) # [N, K]

        # Update student
        loss = get_loss(t1, s2) + get_loss(t2, s1)
        loss.backward()
        optimizer.zero_grad()
        optimizer.step()

        # Update teacher
        gt.param = l * gt.param + (1 - l) * gs.param
        c = m * c + (1 - m) * torch.cat([t1, t2]).mean(0)

def get_loss(t, s):
    t = t.detach() # [N, K]
    s = torch.softmax(s / s_temperature, dim=1) # [N, K]
    t = torch.softmax((t - c) / t_temperature, dim=1) # [N, K]
    return -(t * torch.log(s)).sum(dim=1).mean()
```

数据增强是随机裁减，大于 50% 占比面积的图像缩放到 224 送入教师网络，小于 50% 占比面积的图像缩放到 96 送入学生网络。

为了避免模式坍缩，训练教师网络的时候还有两个额外的操作：
1. 中心化：防止某个维度占主导，但是倾向于变成均匀分布，具体操作和 Batch Normalization 类似。
2. 锐化：与中心化相反，就是 temperature 的作用。
两个操作一起进行。
