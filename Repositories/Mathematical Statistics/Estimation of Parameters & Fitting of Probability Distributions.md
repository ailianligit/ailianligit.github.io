# Estimation of Parameters & Fitting of Probability Distributions

## The Method of Moments

- 抽样分布：参数的估计量的概率分布（参数的估计标准误差）
  - 抽样调查中的抽样分布：样本均值、总数或比例的抽样分布（估计方差）
  - 假设检验中的抽样分布：检验统计量的概率分布
- k阶样本矩：$\widehat{\mu}_k=\frac{1}{n}\sum^{n}_{i=1}X_i^k$
- 估计标准误差：使用估计的参数求标准误差
  - 当样本容量趋于无穷时，参数估计趋近于参数；只要标准误差函数关于参数连续，就有估计标准误差趋近于标准误差
- 矩估计的一致性：当样本容量趋于无穷时，估计参数依概率收敛于参数的真实值



## The Method of Maximum Likelihood

- 容量为n的**独立同分布**样本，似然函数：$\text{lik}(\theta)=\prod ^n_{i=1}f(X_i|\theta)$，对数似然：$l(\theta)=\sum^n_{i=1}\log f(x_i|\theta)$
- 令对数似然的一阶导数为0，该方程可能没有闭式解，因此需要用迭代的方法求解，迭代开始之前，可以选用矩估计作为初始值


- 自助法用于近似参数的抽样分布：用MOM或MLE代替



### Maximum Likelihood Estimates of Multinomial Cell Probabilities

- $f(x_1,\dots,x_m|p_1,\dots,p_m)=\frac{n!}{\prod_{i=1}^m x_i!}\prod_{i=1}^m p_i^{x_i}$，所有单元概率之和为1，使用拉格朗日乘子法最大化对数似然，得到$\widehat{p}_j=\frac{x_j}{n}$
- 单元概率是其他未知参数的函数，$p_i=p_i(\theta)$




### Large Sample Theory for Maximum Likelihood Estimates

$$
I(\theta)=E[\frac{\partial}{\partial\theta}\log f(X|\theta)]^2=-E[\frac{\partial^2}{\partial\theta^2}\log f(X|\theta)]
$$

$$
\sqrt{nI(\widehat{\theta})}(\widehat\theta-\theta_0)\sim N(0,1)
$$

- 独立同分布下，$Var(\widehat{\theta})=\frac{1}{nI(\theta_0)}$，使用估计的参数替换真实的参数，$I(\theta)$通常选取二阶偏导表达式
- 非独立同分布下，$Var(\widehat{\theta})=\frac{1}{E[l'(\theta_0)^2]}=-\frac{1}{E[l''(\theta_0)^]}$，不同于独立同分布，该式需要计算对数似然，一般都是使用第二个等号
- 独立同分布样本的最大似然估计的一致性：当样本容量趋于无穷时，参数估计依概率收敛于参数的真实值



### Confidence Intervals from Maximum Likelihood Estimates

- 精确法：参数的实际分布
- 基于最大似然估计大样本性质的近似法：参数的抽样分布近似正态
  - 独立同分布：$$Var(\widehat{\theta})=\frac{1}{nI(\widehat{\theta})}$$
  - 非独立同分布：$Var(\widehat{\theta})\approx\frac{1}{E[l'(\widehat{\theta})^2]}=-\frac{1}{E[l''(\widehat{\theta})^]}$
- 自助置信区间法



## The Bayesian Approach to Parameter Estimation

- $f_{\Theta|X}(\theta|x)=\frac{f_{X|\Theta}(x|\theta)f_\Theta(\theta)}{\int f_{X|\Theta}(x|\theta)f_\Theta(\theta)d\theta}$
- Posterior Mean: 后验分布的均值
- Posterior Mode: 后验分布的最大值点
- 贝叶斯估计的标准差：后验分布的标准差



## Efficiency and the Cramér-Rao Lower Bound

- Efficiency: $\text{eff}(\widehat{\theta},\tilde{\theta})=\frac{Var(\tilde{\theta})}{Var(\widehat{\theta})}$
  - If use asymptotic variance, then **asymptotic relative efficiency**
- Cramér-Rao Inequality: $Var(T)\ge\frac{1}{nI(\theta)}$
  - $X_1,\cdots,X_n$独立同分布，$T=t(X_1,\cdots,X_n)$是参数$\theta$的无偏估计
  - 最大似然估计的渐近方差达到了该下界，因此最大似然估计是渐近有效的



## Sufficiency

- 充分性：在给定统计量$T=t$的情况下，$X_1,\cdots,X_n$的条件分布对于任何$t$值都不依赖于参数$\theta$

- 充分统计量中包含了数据$x_1,\cdots,x_n$中所有$\theta$的信息；最大似然估计是充分统计量的函数

- 因子分解定理：$T(X_1,\cdots,X_n)$为$\theta$充分统计量的充要条件是以下分解：
  $$
  f(x_1,\cdots,x_n|\theta)=g[T(x_1,\cdots,x_n),\theta]h(x_1,\cdots,x_n)
  $$



## 大样本理论

- 当样本数趋于无穷时，样本均值的抽样分布近似服从均值为$\mu$，方差为$\sigma_{\overline{X}}$的正态分布
- 样本均值的估计标准误差：$s_{\overline{X}}$
- 泊松分布的参数$\lambda$趋于无穷时，泊松分布近似正态分布，均值和方差为泊松分布的均值和方差
- 最大似然估计的参数的抽样分布近似服从均值为$\theta_0$，方差为$\frac{1}{nI(\theta_0)}$的正态分布
- 最大似然估计的渐近方差：$Var(\widehat{\theta})$
- 当样本容量趋于无穷时，$-2\log\Lambda$的零分布趋于自由度为$\dim\Omega-\dim\omega_0$的卡方分布
