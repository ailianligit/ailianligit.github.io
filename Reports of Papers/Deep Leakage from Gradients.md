# [DLG@NeurIPS'21] Deep Leakage from Gradients

## 总结

- 提出问题：共享梯度可能会泄露私人训练数据
- Deep Leakage from Gradients（DLG）：恢复图像pixel-wise精确，恢复文本token-wise匹配
- 解决方法
  - 梯度加噪（扰动）：高斯噪声和拉普拉斯噪声、半精度
  - **梯度压缩和稀疏化**
  - 更大的batch，更高的分辨率和密码学



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



## 实验

- 图像分类
- 掩码语言模型



## 防御策略

- 梯度加噪（梯度扰动）：高斯噪声和拉普拉斯噪声、半精度
- **梯度压缩和稀疏化**：有效
- 更大的batch，更高的分辨率和密码学