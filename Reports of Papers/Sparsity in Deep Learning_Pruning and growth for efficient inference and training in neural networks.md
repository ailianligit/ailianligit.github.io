# [Tutorial@ICML'21] Sparsity in Deep Learning: Pruning and growth for efficient inference and training in neural networks

## 摘要

深度学习不断增长的能源和性能成本促使社区通过选择性修剪组件来减小神经网络的规模。 与它们的生物对应物类似，稀疏网络的泛化能力与原始密集网络一样好，甚至更好。 稀疏性可以减少常规网络的内存占用以适应移动设备，并缩短不断增长的网络的训练时间。 在本文中，我们调查了深度学习中关于稀疏性的先前工作，并为推理和训练提供了广泛的稀疏化教程。 我们描述了删除和添加神经网络元素的方法、实现模型稀疏性的不同训练策略，以及在实践中利用稀疏性的机制。 我们的工作从 300 多篇研究论文中提炼出想法，并为今天希望利用稀疏性的从业者以及旨在推动前沿发展的研究人员提供指导。 我们包括稀疏化数学方法的必要背景，描述早期结构适应、稀疏性和训练过程之间的复杂关系等现象，并展示在真实硬件上实现加速的技术。 我们还定义了一个修剪参数效率的度量，可以作为比较不同稀疏网络的基线。 最后，我们推测稀疏性如何改善未来的工作量并概述该领域的主要开放问题。



## 绪论

### 1.1 模型压缩技术绪论

![image-20230509173115462](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230509_1683624678.png)

![image-20230509173805221](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230509_1683625087.png)



### 1.2 背景和注释

![image-20230509173325576](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230509_1683624807.png)

#### 1.2.4 基于设计稀疏性的卷积层

![image-20230509173537328](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202305/20230509_1683624938.png)