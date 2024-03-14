# Self-Supervised Learning

视觉的自监督学习最主要的任务就是如何只用图像就产生合适的自监督信号。合适的信号不仅能够指导模型的预训练，还能无缝地和下游任务地调优衔接。视觉的自监督学习分成两大流派，对比学习（Contrastive Learning）和生成学习（Generative Learning）。

但是我隐约感到，Scaling Law 在这之中占的分量还是特别重。

## Contrastive Learning

对比学习主要是找到同一张图像在多个不同的视角下的最大公约数。对比学习最主流的流派是设计各种数据增强的方式。

SimCLR

MoCo v3

DINO

## Generative Learning

生成学习最主流的流派是图像补全模型（Masked Image Modeling）。此外还有位置预测（Position Prediction）。

### Masked Image Modeling

#### 2021 BEiT: Bidirectional Encoder representation from Image Transformers (ICLR 2022)

图像除了 patch 的形式，还有 vision token 形式。Vision token 使用 VQ-VAE 之类的离散潜空间 VAE 得到的。BEiT 输入一张图片，分成 patch，随机 mask 40% 的patch，全部输入 ViT，输出被 mask 的 patch 的 VQ-VAE 字母。

#### [2021 Mask Autoencoder Are Scalable Vision Learners (CVPR 2022)](2021%20Mask%20Autoencoder%20Are%20Scalable%20Vision%20Learners.md)

FAIR、何恺明。敢于把 mask 率提高到 75%，且在 encoder 阶段丢弃被 mask 的 patch，形式特别简单。

#### [2022 BEiT v2: Masked Image Modeling with Vector-Quantized Visual Tokenizers (ICLR 2023)](2022%20BEiT%20v2%20Masked%20Image%20Modeling%20with%20Vector-Quantized%20Visual%20Tokenizers.md)

之前 VQ-VAE 是做重建任务，现在是用 CLIP、DINO 做指导。

### Position Prediction

#### [2023 DropPos: Pre-Training Vision Transformers by Reconstructing Dropped Positions (Neurips 2023)](2023%20DropPos%20Pre-Training%20Vision%20Transformers%20by%20Reconstructing%20Dropped%20Positions.md)

国科大、自动化所。计算量更小，但是效果下降了。

## Related Works

这里记录了一些可能需要关注的工作。

- 2022 Position prediction as an effective pretraining strategy.
- 2022 Representation Learning by Detecting Incorrect Location Embeddings

## Rank

这里记录了各种模型在各种数据集上的排名。

### ImageNet-1K

|  Model  |    Type     | Scale | Score | Epoch |
| :-----: | :---------: | :---: | :---: | :---: |
| BEiT-v2 | Generative  | ViT-L | 87.3  |  300  |
|   MAE   | Generative  | ViT-L | 85.7  | 1600  |
| BEiT-v2 | Generative  | ViT-B | 85.5  |  300  |
| BEiT-v1 | Generative  | ViT-L | 85.2  |  800  |
|   MAE   | Generative  | ViT-L | 84.8  |  800  |
| MoCo-v3 | Contrastive | ViT-L | 84.1  |  600  |
|   MAE   | Generative  | ViT-B | 83.6  |  800  |
| BEiT-v1 | Generative  | ViT-B | 83.2  |  800  |
| DropPos | Generative  | ViT-L | 83.2  |  800  |
|  DINO   | Contrastive | ViT-B | 83.2  | 1600  |
|   ViT   |  Supervise  | ViT-L | 73.8  |  50   |

### COCO Detection

