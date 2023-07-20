# [HarsanyiNet@ICML'23] HarsanyiNet: Computing Accurate Shapley Values in a Single Forward Propagation

## 摘要

- HarsanyiNet 对输入样本进行推断，并同时计算单次前向传播中输入变量的**精确** Shapley 值

- HarsanyiNet 的设计理论基础是 Shapley 值可以重新表述为网络编码的 Harsanyi 相互作用的重新分布



## 绪论

- 输入变量的shapley值：输入变量的由 DNN 编码的不同 Harsanyi 交互的重新分布
  - 给定 DNN 和具有 n 个变量的输入样本，Harsanyi 交互$S$表示$S$中变量之间的 AND 关系，由 DNN 编码
  - 一个 DNN 通常编码很多不同的 Harsanyi 交互
  - 每个 Harsanyi 交互对 DNN 的推理得分做出特定的数值贡献

- HarsanyiNet 优势：精确的shapley value
  - 有理论保证和实验验证
  - 不限制特定交互的表示，保证了更宽阔的应用性，展示了更好的性能
- 贡献
  - HarsanyiNet: simultaneously perform **model inference** and **compute exact Shapley values** in one forward propagation
  - Following the paradigm of HarsanyiNet, we design Harsanyi-MLP and Harsanyi-CNN for tabular data and image data, respectively
  - The HarsanyiNet does not constrain **the representation of specific interactions**, but it can still guarantee **the accuracy of Shapley values**



## 相关工作

- 计算输入变量的归因
  - 梯度
  - 归因的反向传播
  - 输入变量的扰动
- shapley值
- 计算复杂度与近似精度的困境
  - 估计方法：更高的估计精度需要更多的网络推理
  - 采样技术
  - 将估计转换为一个加权最小二乘问题
  - 假设一个特定的数据分布
  - 忽略输入变量之间的微小的交互
  - 学习一个解释器模型来直接预测shapley值
  - ShapNet能够编码少数变量之间的交互，准确估计shapley值



## 方法论

### 预备知识：Harsanyi 交互