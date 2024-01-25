# Self-Supervised Learning

视觉的自监督学习最主要的任务就是如何只用图像就产生合适的自监督信号。合适的信号不仅能够指导模型的预训练，还能无缝地和下游任务地调优衔接。视觉的自监督学习分成两大流派，对比学习（Contrastive Learning）和生成学习（Generative Learning）。

## Contrastive Learning

对比学习主要是找到同一张图像在多个不同的视角下的最大公约数。对比学习最主流的流派是设计各种数据增强的方式。

## Generative Learning

生成学习最主流的流派是图像补全模型（Masked Image Modeling）。此外还有位置预测（Position Prediction）。

### Masked Image Modeling

#### [2021 Mask Autoencoder Are Scalable Vision Learners (CVPR 2022)](2021%20Mask%20Autoencoder%20Are%20Scalable%20Vision%20Learners.md)

FAIR、何恺明

### Position Prediction

#### [2023 DropPos: Pre-Training Vision Transformers by Reconstructing Dropped Positions (Neurips 2023)](2023%20DropPos%20Pre-Training%20Vision%20Transformers%20by%20Reconstructing%20Dropped%20Positions.md)

国科大、自动化所

## Related Works

这里记录了一些可能需要关注的工作。

- 2022 Position prediction as an effective pretraining strategy.
- 2022 Representation Learning by Detecting Incorrect Location Embeddings

## Rank

这里记录了各种模型在各种数据集上的排名。

### ImageNet-1K

| Model | Genre | Scale | Score | Epoch |
| :--: | :--: | :--: | :--: | :--: |
| MAE | Generative | ViT-L | 85.9 | 1600 |
| DropPos | Generative | ViT-L | 85.8 | 800 |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |

### COCO Detection




