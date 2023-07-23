# [OUTPOST@INFOCOM'23] More than Enough is Too Much: Adaptive Defenses against Gradient Leakage in Production Federated Learning

## 总结

- 解决问题：在production FL中研究梯度泄露攻击，设计一个实用、简单、轻量级的防御方法
- 重要发现：在production FL中许多假设都不成立
  - **model updates** ≠ gradients
  - There are **multiple update** steps (across batches and epoches) in local training
    - $\tau=E \cdot n / B$
    - 大多数假设：$B=n\ge1, E=1$
    - Production FL：$n> 1, B<n, E>1$
  - Label estimation is more challenging with **non-i.i.d**. data distributions
  - 神经网络模型中的**初始权重**极大地影响了执行梯度反转攻击的难度

![image-20230713211714571](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230714_1689307925.png)

- Outpost
  - **轻量级**防御机制，针对随时间变化的隐私泄露风险级别提供**充分且自适应的保护**
  - 基于magnitudes进行**梯度压缩**
  - 根据隐私泄露风险向梯度添加**高斯噪声**，隐私泄露风险通过**每层模型权重的分布**进行量化，并通过**经验Fisher矩阵**选择添加噪声的梯度
  - 为了限制计算开销和训练性能下降，Outpost仅执行**基于迭代的衰减的扰动**
- 实验效果：在时间开销、准确率和隐私保护方面实现最佳的权衡



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

- 隐私泄露的风险：the magnitude of neural network weights -> the privacy leakage risks of the corresponding gradients
- 选择性扰动：empirical Fisher for each model parameter

$$
\widehat{\mathbf{F}}=\frac{1}{n / B} \sum_{i=1}^{n / B} \nabla \mathcal{L}_{\theta i} \cdot \nabla \mathcal{L}_{\theta i}^{\top}
$$

- 基于迭代的扰动衰减：$\text{Pr}=1/(1+\beta\cdot i)$

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230714_1689304223.png" alt="image-20230714111021836" style="zoom: 50%;" />



## 实验结果

- 训练性能：accuracy loss少，time-consuming少

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230714_1689305535.png" alt="image-20230714113213675" style="zoom:50%;" />

- 防御性能
  - Performing the attack on a **larger batch size** not only makes the attacks significantly slower, but also worse
  - **MSE** is the most inconsistent metric and a higher MSE does not correlate to a reconstructed image containing fewer key features of ground truth



## 相关工作

- 梯度泄露攻击：DLG、iDLG（只需要恢复输入）、Inverting Gradients（余弦相似度）、GradInversion（更高的fidelity和更好的localization）、GIAS（关于预训练生成模型的先验知识）
- 梯度泄露攻击的防御方法：梯度压缩、本地差分隐私、Soteria（对全连接层中的数据表示添加扰动）、GradDefense（扰动所有层）、GradDefense（灵敏度测量和所有层扰动）
