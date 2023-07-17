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
s_{i}=\frac{1}{N !} \sum_{\pi \in \Pi(D)}\left[U\left(P_{i}^{\pi} \cup\{i\}\right)-U\left(P_{i}^{\pi}\right)\right]
$$

- Properties
  - Group Rationality (Efficiency)
  - Fairness (Symmetry & Null player)
  - Additivity (Linearity)
- Complexity: $O(2^N)$



## 高效的SV估计

- 基线：排列采样
- 基于group测试的方法