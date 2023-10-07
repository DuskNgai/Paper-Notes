# A Foundation Model for Generalizable Disease Detection from Retinal Images

Nature 正刊的切入点会特别大。

## 0 Abstract

医疗 AI 在诊断方面潜力巨大，特别是视网膜的图像的诊断。作者认为当前的难题在于数据标注，以及 domain-specific/task-specific 的数据使得其泛化性较低。因此提出视网膜图像的基础模型 RETFound，其本质就是一个 MAE 模型。那该如何包装这个基础模型呢？就得应用在各种下游任务中，作为便于标记任务的下游任务的基础模型。

## 1 Introduction  

第一段的切入点是，深度神经网络在视网膜图像和 X 光图像分析的准确性方面达到了人类专家的水平。但是人类专家是稀缺的，人类专家来标记图像成本也是非常高昂的，因此开发医疗 AI 辅助是非常有必要的。

第二段讲述自监督模型。自监督模型是从数据中直接获取数据的特征，可以用在大量无标记数据上，比有监督模型更容易进行泛化。训练好了无监督模型之后就可以应用到下游任务中。随后找 CV 中的案例为其背书，特别是 MAE。因此在海量的无标记医疗图像上的潜力巨大。

第三段讲述视网膜图像的现状。CFP (Color Fundus Photography) 图像和 OCT (Optical Coherence Tomography) 图像数据量很大，而且不仅眼科需要，神经内科也可以分析相关图像，甚至普通内科也有需求。然后就可以讲当前自监督模型的缺陷了：

1. 现有的自监督模型还不能泛化到每个需求领域中。
2. 视网膜图像的大型存储库的建立存在挑战。
3. 自监督模型，不管是对比学习还是生成方法，能力和解释性都不足。但退一步讲，特定特征的可解释性是安全可靠地临床落地的重要一步。

第四段就提出了 RETFound，给出了基础模型的定义，针对 CFP (904170) 和 OCT (736442) 各训练了一个 MAE，并将其用在了各种下游任务（疾病监测）：

1. 眼病诊断分类：糖尿病视网膜病变、青光眼。
2. 眼病预后：一年内对侧眼睛向新生血管性年龄相关性黄斑变性的转变。
3. 内科疾病：三年内心血管疾病(缺血性中风、心肌梗死和心力衰竭)和神经退行性疾病(帕金森病)的预测。

作者表示，RETFound 比迁移学习的模型能更好的反映眼科学的相关领域知识。

第五段描述了数据集的来源，以及下游任务的选择。

第六段描述了 RETFound 的 3 个对比模型，每个模型训练方式不同，但架构和在下游任务的调优策略是一致的。

1. SL-ImageNet：在 ImageNet-21k 上预训练，然后迁移学习。
2. SSL-ImageNet：在 ImageNet-1k 上预训练。
3. SSL-Retinal：在视网膜数据集上预训练。
4. RETFound：用 SSL-ImageNet 的权重再在视网膜数据集上训练。

除了 MAE，还尝试了其他的基础模型（SimCLR, SwAV, DINO, MoCo-v3）。

## 2 Result

### 2.1 Ocular Disease Diagnosis

### 2.2 Ocular Disease Prognosis

### 2.3 Systemic Diseases Predition

### 2.4 Label Efficiency for Disease Detection

### 2.5 SSL Strategies for RETFound

### 2.6 Model Interpretation

### 2.7 Robustness to Age Distribution Shifts

