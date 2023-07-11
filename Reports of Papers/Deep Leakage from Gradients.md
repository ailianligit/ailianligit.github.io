# [DLG@NeurIPS'21] Deep Leakage from Gradients

## 摘要

- the recovery is **pixelwise accurate** for images and **token-wise matching** for texts
- most effective defense method is **gradient pruning**



## 绪论

- distributed learning system

  - [parameter server](https://zhuanlan.zhihu.com/p/82116922)

  - [all-reduce](https://zhuanlan.zhihu.com/p/100012827)

- Deep Leakage from Gradients (DLG): sharing the gradients can leak private training data

  - only requires the gradients
  - can reveal pixel-wise accurate images and token-wise matching texts

- defense strategies

  - **gradient perturbation**
  - low precision
  - **gradient compression**



## 相关工作

- 分布式训练
- “Shallow” Leakage from Gradients



## 方法

![image-20230711183641291](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230711_1689071802.png)

![image-20230711183740988](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230711_1689071862.png)