# De Novo Design of Protein Interactions with Learned Surface Fingerprints

## 0 Abstract

作者强调，理解蛋白质在生物学过程中的相互作用非常重要，但是当前这一领域存在知识空白。随后，作者介绍了一种基于蛋白质表面的几何深度学习框架，用于设计新的蛋白质相互作用。通过设计针对特定蛋白质目标的蛋白质结合剂，这种方法证明了其在预测分子识别的物理和化学决定因素方面的准确性，展示了一种用于合成生物学和医疗应用的蛋白质相互作用设计的全新途径。

## 1 Introduction

第一段描述问题。强调了在计算蛋白质设计领域中创造新的蛋白质-蛋白质相互作用的重要挑战及其在生物学基础研究和转化应用中的广泛重要性。它指出，生成特定绑定目标位点的氨基酸序列的任务，是对我们理解生物分子相互作用的物理化学原则的一项严峻测试。开发用于设计新型 PPI 的可靠计算方法，对于快速工程化开发基于蛋白质的治疗药物等具有重要意义，对生物医药和转化应用领域具有重大兴趣。

第二段描述现状。尽管在蛋白质相互作用设计和预测领域取得了进展，但针对没有已知结构信息的特定目标设计新的蛋白结合剂依然面临重大挑战。现有方法主要通过在目标界面上优化残基的展示，但受限于评分函数提供的能量信号弱和适合的蛋白质框架难以找到。因此，迫切需要开发新的策略来设计能够适应不同表面类型和蛋白位点的新型结合剂。

第三段描述方法。讨论了蛋白质-蛋白质相互作用形成的分子识别模型，强调化学和几何互补性的重要性。介绍了一种新的蛋白质设计方法，侧重于界面的完全埋藏区域，以促进蛋白质间的相互作用。通过学习相互作用表面的特征，该方法能够快速准确地识别用于设计 PPI 的关键位点，并简化了寻找和应用热点区域的过程。成功应用此方法设计针对四个治疗目标的结合剂，展示了其有效性和潜在的应用价值。

## 2 Design Strategy and _in silico_ Validation


