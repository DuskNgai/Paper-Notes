# Panoptic Segmentation

## 0 Abstract

全景分割将**语义分割**（**为每个像素分配一个类别标签**）和**实例分割**（**检测和分割每个物体实例**）这两个通常截然不同的任务统一起来。

## 1 Introduction

**Stuff/Thing：东西/物体。**

我们提出了一项任务：（1）同时包含 stuff 和 object 两个类别，（2）使用简单但通用的输出格式，（3）引入统一的评估指标。为了与之前的工作明确区分，我们将由此产生的任务称为全景分割。全景的定义是“在一张图中包括所有可见的事物”，在我们的语境中，全景指的是分割的统一、全局视图。

我们采用的全景分割任务格式很简单：**为图像的每个像素分配一个语义标签和一个实例 id**。

## 2 Related Work

## 3 Panoptic Segmentation Format

|                         Symbols                          |         Descriptions          |
| :------------------------------------------------------: | :---------------------------: |
|                           $i$                            |           图像像素            |
|             $\mathcal{L} := \{0,\dots,L-1\}$             |     $L$ 个语义标签组成的      |
|                   $l_i\in\mathcal{L}$                    | 像素 $i$ 的语义标签语义标签集 |
|                    $z_i\in\mathbb{N}$                    |      像素 $i$ 的实例 id       |
| $\mathcal{L}^{\mathrm{St}}$, $\mathcal{L}^{\mathrm{Th}}$ |      东西标签和事物标签       |

### Task format

给定一组 $L$ 个语义标签的编码 $\mathcal{L}$ ，任务要求全景分割算法将图像中的每个像素映射到其语义标签和实例 id 的组合中。
$$
\mathrm{PS}:\mathbb{N}\mapsto\mathcal{L}\times\mathbb{N}\Longrightarrow\mathrm{PS}(i)=(l_i, z_i)
$$
$z_i$ 将同一语义标签的像素分成不同的 id。GT 注释的方式相同。模糊的或 $L$ 个语义标签外的像素可被分配一个特殊的空标签；也就是说，并非所有像素都必须有一个语义标签。

### Stuff and thing labels

语义标签集由这两个子集组成，且 $\mathcal{L}=\mathcal{L}^{\mathrm{St}}\cap\mathcal{L}^{\mathrm{Th}}$、 $\mathcal{L}^{\mathrm{St}}\cup\mathcal{L}^{\mathrm{Th}}=\emptyset$。当像素被标记为 $l_i\in\mathcal{L}^{\mathrm{St}}$ 时，其对应的实例 id $z_i$ 是无关紧要的。对于 stuff 类来说，所有像素都属于同一个实例（如同一片天空）。对于 thing 类来说，所有具有相同 $(l_i, z_i)$ 的像素都属于同一个实例（例如，同一辆汽车）；反之，属于单个实例的所有像素必须具有相同的 $(l_i,z_i)$。和以前的数据集一样，选择哪些类别是 stuff 还是 thing 是由数据集创建者自行决定的。

### Relationship to semantic segmentation





## 4 Panoptic Segmentation Metric

