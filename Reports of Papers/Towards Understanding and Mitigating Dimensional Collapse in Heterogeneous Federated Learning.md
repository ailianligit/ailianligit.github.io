# [FedDecorr@ICLR'23] Towards Understanding and Mitigating Dimensional Collapse in Heterogeneous Federated Learning

## 摘要

- 联邦学习一个主要挑战是**数据异质性**问题，它指的是不同客户端之间的本地数据分布之间的差异。
- 为了解决这个问题，我们首先研究数据异质性如何影响全局聚合模型的 representations。
  - 发现异构数据导致全局模型遭受严重的**维度崩溃**，其中 representations 倾向于驻留在较低维度的空间而不是周围的空间中。
  - 在每个客户端本地训练的模型上观察到类似的现象，并推断出全局模型的维度崩溃是从本地模型继承的。
  - 从理论上分析了**梯度流动力学**，以阐明数据异质性如何导致局部模型的维度崩溃。
- FedDecorr：一种可以有效减轻联邦学习中维度崩溃的新方法。
  - 具体来说，FedDecorr 在局部训练期间应用正则化项，鼓励 representations 的不同维度不相关。
  - FedDecorr 易于实现且计算效率高，在标准基准数据集上对基线产生了一致的改进。



## 相关工作

- 联邦学习数据异构
  - 提升本地训练：高计算或通信开销
  - 个性化联邦学习：与本文目标不同
- 维度崩溃
  - 度量学习、自监督学习、传统增量学习
- 梯度流动力学
  - 分析多层线性神经网络在L2损失下的动力学，发现在优化过程中更深层的神经网络偏向于低秩解。
- 特征去相关（Feature Decorrelation）
  - 解决联邦学习中由数据异质性导致的维度崩溃



## 绪论

- 数据异质性问题中的标签分布的异质性
  - 导致客户端的局部最优值与所需的全局最优值之间出现严重分歧
  - 可能导致全局模型的性能严重下降
- 数据异质性的工作集中在模型参数上
  - 局部训练：[FedProx](https://arxiv.org/abs/1812.06127)，[SCAFFOLD](https://arxiv.org/abs/1910.06378)
  - 全局聚合：[FedNova](https://arxiv.org/abs/2007.07481)
  - 缺点：过多的计算负担或高昂的通信成本，因为深度神经网络通常会严重过度参数化
- 异构数据如何影响联邦学习中的全局模型
  - 研究每个全局模型的 representations 输出：**协方差矩阵的奇异值**
  - 数据异质性👉**维度崩溃**（dimensional collapse）
  - 维度崩溃：模型的一种过度简化形式，表示空间没有被充分利用来区分不同类别的数据
- 全局模型的维度崩溃从在各种客户端上本地训练的模型继承而来
  - 本地训练的梯度流动力学（gradient flow dynamics）
  - 异构数据驱动局部模型的权重矩阵偏向**低秩**

![image-20230525132523504](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230525_1684992325.png)

- FedDecorr
  - 在**局部训练**期间添加了一个**正则化项**
  - 鼓励 **representations 的相关矩阵的 Frobenius 范数**更小

- 贡献
  - 通过实验发现联邦学习中更强的数据异质性会导致全局和局部模型更大的维度崩溃
  - 对将数据异质性和维度崩溃联系起来的实证发现背后的动力学发展了理论的理解
  - 基于减轻维度崩溃的动机，提出了FedDecorr，实现友好且计算效率高



## 由数据异质性造成的维度崩溃

![image-20230525141551981](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230525_1684995353.png)

### 全局模型上的实证发现

### 本地模型上的实证发现

### 对于维度崩溃的理论解释



## 通过FedDecorr缓解维度崩溃

![image-20230525150458008](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230525_1684998299.png)

![image-20230525150940577](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230525_1684998581.png)

![image-20230525150018597](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230525_1684998020.png)

![image-20230525150057064](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230525_1684998058.png)
