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