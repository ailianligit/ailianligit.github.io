# [FedDecorr@ICLR'23] Towards Understanding and Mitigating Dimensional Collapse in Heterogeneous Federated Learning

## Abstract

- 联邦学习一个主要挑战是**数据异质性**问题，它指的是不同客户端之间的本地数据分布之间的差异。
- 为了解决这个问题，我们首先研究数据异质性如何影响全局聚合模型的representations。
  - 发现异构数据导致全局模型遭受严重的**维度崩溃**，其中representations倾向于驻留在较低维度的空间而不是周围的空间中。
  - 在每个客户端本地训练的模型上观察到类似的现象，并推断出全局模型的维度崩溃是从本地模型继承的。
  - 从理论上分析了**梯度流动力学**，以阐明数据异质性如何导致局部模型的维度崩溃。
- FedDecorr：一种可以有效减轻联邦学习中维度崩溃的新方法。
  - 具体来说，FedDecorr在局部训练期间应用正则化项，鼓励representations的不同维度不相关。
  - FedDecorr易于实现且计算效率高，在标准基准数据集上对基线产生了一致的改进。



## Introduction

- 数据异质性问题中的标签分布的异质性
  - 导致客户端的局部最优值与所需的全局最优值之间出现严重分歧
  - 可能导致全局模型的性能严重下降
- 数据异质性的工作集中在模型参数上
  - 局部训练：[FedProx](https://arxiv.org/abs/1812.06127)，[SCAFFOLD](https://arxiv.org/abs/1910.06378)
  - 全局聚合：[FedNova](https://arxiv.org/abs/2007.07481)
  - 缺点：过多的计算负担或高昂的通信成本，因为深度神经网络通常会严重过度参数化
