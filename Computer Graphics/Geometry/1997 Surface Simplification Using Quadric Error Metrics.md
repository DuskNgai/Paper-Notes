# Surface Simplification Using Quadric Error Metrics

## 0 Abstract

实际需要的详细程度可能有很大的不同。为了控制处理时间，通常希望使用近似值来代替过于详细的模型。
The level of detail actually necessary may vary considerably. To control processing time, it is often desirable to use approximations in place of excessively detailed models.

该算法使用顶点对的迭代收缩来简化模型，并使用四边形矩阵维持表面误差近似值。通过收缩任意的顶点对（而不仅仅是边），我们的算法能够连接模型中不相连的区域。这可以促进更好的近似，无论是在视觉上还是在几何误差方面。为了允许拓扑连接，我们的系统也支持非流形表面模型。
The algorithm uses iterative contractions of vertex pairs to simplify models and maintains surface error approximations using quadric matrices. By contracting arbitrary vertex pairs (not just edges), our algorithm is able to join unconnected regions of models. This can facilitate much better approximations, both visually and with respect to geometric error. In order to allow topological joining, our system also supports non-manifold surface models.

## 1 Introduction

我们将假设模型仅由三角形组成。这并不意味着丧失一般性，因为原始模型中的每一个多边形都可以作为预处理阶段的一部分被三角化。为了获得更可靠的结果，当两个面的角相交于一点时，面应该被定义为共享一个顶点，而不是使用两个单独的、恰好在空间上重合的顶点。
We will assume that the model consists of triangles only. This implies no loss of generality, since every polygon in the original model can be triangulated as part of a preprocessing phase. To achieve more reliable results, when corners of two faces intersect at a point, the faces should be defined as sharing a single vertex rather than using two separate vertices which happen to be coincident in space.

我们的算法是基于顶点对的迭代收缩（边缘收缩的一般化）。随着算法的进行，在当前模型的每个顶点都保持着一个几何误差近似值。这个误差近似值用四边形矩阵表示。
Our algorithm is based on the iterative contraction of vertex pairs (a generalization of edge contraction). As the algorithm proceeds, a geometric error approximation is maintained at each vertex of the current model. This error approximation is represented using quadric matrices.

## 2 Background and Related Work

许多先前的简化算法都隐含地或明确地假定他们的输入曲面是，而且应该是流形曲面。我们要强调的是，我们不做这种假设。事实上，聚合的过程中会经常产生非流形区域。
Many prior simplification algorithms have either implicitly or explicitly assumed that their input surfaces were, and ought to remain, manifold surfaces. Let us stress that we do not make this assumption. In fact, the process of aggregation will regularly create non-manifold regions.

### 2.1 Surface Simplification

#### Vertex Decimation

Their method iteratively selects a vertex for removal, removes all adjacent faces, and re-triangulates the resulting hole.



## 3 Decimation via Pair Contraction









