# [OUTPOST@INFOCOM'23] More than Enough is Too Much: Adaptive Defenses against Gradient Leakage in Production Federated Learning

## 摘要

- 梯度攻击（gradient attacks）对原始数据的隐私构成的威胁是有限的
- 通过在实际假设下对联邦学习系统中现有的梯度攻击进行综合评估，我们系统地分析了它们在各种配置下的有效性
- 我们提出了使攻击成为可能或更强所需的关键**先验**，例如**初始模型权重的狭窄分布**，以及**训练早期阶段的反转**
- OUTPOST
  - **梯度扰动（gradient perturbation）**方法的一种变体
  - **轻量级**防御机制，针对随时间变化的隐私泄露风险级别提供**充分且自适应的保护**
  - 根据 **Fisher 信息矩阵**在每次更新迭代时有选择地向梯度添加**高斯噪声**，其中噪声水平由**隐私泄露风险（privacy leakage risk）**决定，隐私泄露风险通过**每层模型权重的分布（spread）**进行量化
  - 为了限制计算开销和训练性能下降，OUTPOST 仅执行**基于迭代的衰减的扰动（perturbation with iteration-based decay）**
- 实验结果表明，在**收敛性能**、**计算开销**和**防止梯度攻击**方面，OUTPOST 可以比最先进的技术实现更好的权衡



## 绪论

- [Deep Leakage from Gradients (DLG)](https://arxiv.org/abs/1906.08935): reconstruction of raw private data
- 现有工作在假设上的问题
  - model updates ≠ gradients
  - full gradient descent
  - neglect the change of model status through FL training
  - 防御之前忽视由 DLG 攻击造成的数据重建的可用性（feasibility）
- OUTPOST的原理
  - 通过模型每一层权重的**方差统计量**评估当前本地模型的隐私泄露风险
  - 通过量化**隐私泄露风险**决定 Fisher 信息矩阵的范围
  - 基于 **Fisher 信息矩阵**往当前 step 梯度的每一层中添加**高斯噪声**
- 实验
  - 效用指标：收敛需要的 wall-clock 时间和收敛的准确率
  - 隐私指标：在最坏情况下抵御梯度泄露攻击的防御的有效性



## 预备知识

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230713_1689253536.png" alt="image-20230713210535043" style="zoom:50%;" />



## 在Production FL中重新评估梯度泄露攻击

### A. 假设：在Production FL中重新评估有效性

- Gradients are not shared directly with the server
- Neural network models are not initialized explicitly before training
  - 神经网络模型中的初始权重极大地影响了执行梯度反转攻击的难度

![image-20230713211714571](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230713_1689254236.png)

- There are multiple update steps (across batches and epoches) in local training
  - $\tau=E \cdot n / B$
  - 大多数假设：$B=n\ge1, E=1$
  - Production FL：$n> 1, B<n, E>1$
- Label estimation is more challenging with non-i.i.d. data distributions

### B. 更加复杂的DLG攻击

- Approximating gradients from updates

- **Matching deltas from updates**

![image-20230713215824198](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230713_1689256705.png)



## OUTPOST：轻量化防御方案