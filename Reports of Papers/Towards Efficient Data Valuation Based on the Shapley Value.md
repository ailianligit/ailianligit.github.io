# [AISTATS'19] Towards Efficient Data Valuation Based on the Shapley Value

## 摘要

## 绪论

![image-20230716220138201](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230716_1689516099.png)

## 相关工作

- feature selection
- understanding model behaviors
- data valuation: **fairness**



## 问题定义

- Shapley value

$$
s_{i}=\sum_{S \subseteq I \backslash\{i\}} \frac{1}{N\left(\begin{array}{c}
N-1 \\
|S|
\end{array}\right)}[U(S \cup\{i\})-U(S)]
$$

$$
s_{i}=\frac{1}{N !} \sum_{\pi \in \Pi(D)}\left[U\left(P_{i}^{\pi} \cup\{i\}\right)-U\left(P_{i}^{\pi}\right)\right]
$$

- Properties
  - Group Rationality (Efficiency)
  - Fairness (Symmetry & Null player)
  - Additivity (Linearity)
- Complexity: $O(2^N)$



## 高效的SV估计

- 基线：排列采样
  - approximates the SV for any **bounded utility functions** with provable guarantees
  - 通过样本均值降低计算shapley value需要的排列数
- 基于group测试的方法
  - 减少评估用户子集utility的次数
  - smartly design the sampling distribution of **the appearance of user's data in a test** such that the expectation of **the difference between the utility of user $i$ and the of user $j$** mirrors **the Shapley difference $s_i-s_j$**
  - first estimates the **Shapley differences** and then derives the SV from the Shapley differences by solving **a feasbility problem**

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230720_1689821666.png" alt="image-20230720105425057" style="zoom:67%;" />

- 利用值的稀疏性
  - SV“近似稀疏”
  - 减少measurement的次数

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230720_1689824292.png" alt="image-20230720113811385" style="zoom: 80%;" />

- 稳定的学习算法
- 基于影响函数的启发式算法
  - training a large number of models on different subsets of data
  - influence function heuristics
  - stratified sampling trick
  - Largest-S Approximation: focuses on the marginal contribution of each training data point to **a single subset**



## 实验结果

- 比较估计准确率
- 运行时间比较

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230720_1689841194.png" alt="image-20230720161948851" style="zoom: 50%;" />

- 在稀疏性假设下的估计

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230720_1689841248.png" alt="image-20230720162044040" style="zoom:67%;" />

- 稳定的学习算法
- 隐私保护数据的值

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230720_1689843405.png" alt="image-20230720165639142" style="zoom: 80%;" />

- 对抗样本的值

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202307/20230720_1689843563.png" alt="image-20230720165917643" style="zoom: 50%;" />

## 结论