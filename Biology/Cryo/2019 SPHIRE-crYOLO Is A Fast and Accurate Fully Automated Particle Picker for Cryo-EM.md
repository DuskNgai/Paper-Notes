# SPHIRE-crYOLO Is A Fast and Accurate Fully Automated Particle Picker for Cryo-EM

> CrYOLO 将图像划分为网格，并预测网格中的每个单元是否包含了（包围着感兴趣物体的边界框的）中心点。如果有，那就预测每个单元内中心点的相对位置和边界框的长和宽。

## 0 Abstract

每个数据集 200 - 2500 个颗粒做训练，然后就能 1 秒 5 张图 ($1024\times1024$)。

## 1 Introduction

颗粒挑选的难点在于：
1. 各异构象
2. 背景、碳边缘作为负样本存在于 Micrograph 中
3. 冰层厚度不一致且有冰晶，很容易与颗粒混淆

相比于滑动窗口方法，CrYOLO
1. 减少了卷积所需要的次数（我觉得没有）
2. 只需要正样本作为训练数据
3. 具有全局的信息

最后在 40 个数据集上训练了一个泛化性比较好的模型。

## 2 Results

#### Convolutional Neural Network

Particle 有时候很小很密集，但原有的 YOLO 在很稀疏的网格上做预测，影响预测的性能。因此就把 Micrograph 分割成互相有所重叠的 $2\times2,3\times3$ 的小图，然后把小图缩放到 $1024\times1024$ 的分辨率。

#### Test Datasets

| EMPIAR | EMDB | Resolution | Name | Training Data | Predicted Data |
| :--: | :--: | :--: | :--: | :--: | :--: |
| 10089 | 3645 | 7475 / 3.5 | TcdA1 | 5 / 1100 | 98 / 10854 |
| 10093 | 8702 | 171917 / 3.4 | NOMPC | 9 / 585 | 1873 / 226289 |
| 10050 | 3233 | 37031 / 4.6 | Prx3 | 5 / 2500 | 802 / 354581 |
| - | 4339 |  | TRPC4 |  |  |
| - | - |  | KLH |  |  |

#### Training and Application of CrYOLO

理想情况下，每张 Micrograph 都应挑选完成。然而，由于对比度较低，用户通常无法选择所有颗粒进行训练，往往会漏掉一些颗粒，即所谓的假阴性。在训练过程中，包含假阴性粒子的惩罚要比遗漏真阳性粒子的惩罚低。IoU > 0.6 就认为是 TP。

虽然挑选出很多的颗粒，但是大部分的颗粒都被丢弃了，没有进入最后的重建阶段。

在合成数据集 TRPC4 上用 AUPRC 衡量不同 SNR 下 crYOLO 的挑选能力。在 SNR 大于 0.041 的情况下性能都不错。

在低噪声 KLH 数据集上衡量训练集大小对于 crYOLO 挑选能力的影响。在低噪声的情况下，1 张 Micrograph 的效果都很好，但是没什么意义。

#### Computational Efficiency

GTX 1080，训练 5 分钟，推理每秒 5 张 Micrograph。

#### Generalization to Unseen Datasets

840 张 Micrograph，包括 26 个人工挑选的数据集、9 个模拟数据集和 10 个仅由污染组成的无颗粒数据集做训练。在 10190 和 10217 上做挑选。AUPRC 基本在 0.85 左右。

## 3 Methods

Loss 为：
$$
\begin{align*}
L&=\sum_{i=0}^{S^{2}}\mathbb{1}_{i}^{\text{obj}}\left[\left(x_{i}-\hat{x}_{i}\right)^{2}-\left(y_{i}-\hat{y}_{i}\right)^{2}\right]\\
&+\sum_{i=0}^{S^{2}}\mathbb{1}_{i}^{\text{obj}}\left[\left(\sqrt{w_{i}}-\sqrt{\hat{w}_{i}}\right)^{2}-\left(\sqrt{h_{i}}-\sqrt{\hat{h}_{i}}\right)^{2}\right]\\
&+\lambda_{\text{obj}}\sum_{i=0}^{S^{2}}\mathbb{1}_{i}^{\text{obj}}\left(C_{i}-\hat{C}_{i}\right)^{2}\\
&+\lambda_{\text{non-obj}}\sum_{i=0}^{S^{2}}\mathbb{1}_{i}^{\text{non-obj}}\left(C_{i}-\hat{C}_{i}\right)^{2}
\end{align*}
$$

其中 $\mathbb{1}_{i}^{\text{obj}}$ 为示性函数，当网格 $i$ 中存在颗粒的中心点时候为 1；$(x_{i},y_{i})$ 和 $(w_{i},h_{i})$ 为 Box 的中心和长宽，$(\hat{x}_{i},\hat{y}_{i})$ 和 $(\hat{w}_{i},\hat{h}_{i})$ 为预测的 Box 的中心和长宽；$C_{i}$ 为网格 $i$ 中存在颗粒的置信度。最后一项增加了一些负样本的惩罚。

数据增强有：
- Gaussian Blurring：$\text{std}\in(0,3)$
- Average Blurring：$\text{Box Size}\in[2,7]$
- Flip：Horizontal & Vertical
- Gaussian Noise：std 和图像的 std 一致。
- Dropout：把 1-10% 的像素用图像的 mean 替换。
- Contrast Normalization：先减去均值，乘上任意的 std，再加上均值。
