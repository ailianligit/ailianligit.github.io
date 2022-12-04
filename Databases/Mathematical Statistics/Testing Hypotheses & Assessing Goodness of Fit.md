# Testing Hypotheses & Assessing Goodness of Fit

## The Bayesian Approach

- Accept if:

$$
\frac{P(H_0|x)}{P(H_1|x)}=\frac{P(H_0)}{P(H_1)}\frac{P(x|H_0)}{P(x|H_1)}>c
$$



## The Neyman-Pearson Paradigm

- 类型Ⅰ错误：当原假设为真时拒绝原假设
  - 其概率称为检验的显著性水平$\alpha$
- 类型Ⅱ错误：当原假设为假时接受原假设
  - 优先减小其概率$\beta$
- 势：当原假设为假时拒绝原假设的概率$1-\beta$
- 零分布：给定原假设下检验统计量的分布
- Neyman-Pearson Lemma: 假设$H_0$和$H_1$是简单假设，检验在似然比小于$c$时拒绝$H_0$，显著性水平是$\alpha$，那么显著性水平小于或等于$\alpha$的任何其他检验都具有小于或等于似然比检验的势
  - The likelihood ratio test is optimal for testing a simple hypothesis versus a simple hypothesis

$$
\Lambda=\frac{lik(\theta_0)}{lik(\theta_1)}
$$



### Specification of the Significance Level and the Concept of a p-value 

- p值：拒绝原假设的最小显著性水平，p值越小，拒绝原假设的证据越充分
  - 给定显著性水平，可以计算检验的拒绝域；若检验统计量落在拒绝域内，则拒绝原假设
  - 计算检验统计量，原假设下统计量大于等于检验统计量的概率为p值
- 统计检验量$X$取较大值时，拒绝原假设（$F(x)$为检验统计量$X$的零分布）

$$
P(X>x|H_0)=\alpha\\p\text{-value}=1-F(x)
$$

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211203201534749.png" alt="image-20211203201534749" style="zoom: 33%;" />



### Uniformly Most Powerful Tests

- 如果备择假设是复杂的，那么关于备择假设中任一简单备择假设都是最优势的检验称为是**一致最优势**的



## The Duality of Confidence Intervals and Hypothesis Tests

- 假设对于$\Theta$中的每个$\theta_0$都有一个显著性水平$\alpha$的假设检验$H_0:\theta=\theta_0$。检验的接受域为$A(\theta_0)$，$\theta$的$100(1-\alpha)\%$置信区间为$C(X)=\{\theta:X\in A(\theta)\}$
- 假设$P[\theta_0\in C(X)|\theta=\theta_0]=1-\alpha$，显著性水平$\alpha$假设检验$H_0:\theta=\theta_0$的接受域为$A(\theta_0)=\{X|\theta_0\in C(X)\}$
- 对于$H_0:\mu=\mu_0$，指定显著性水平$\alpha$的检验接受域与$\mu_0$的$100(1-\alpha)\%$置信区间相同



## Generalized Likelihood Ratio Tests

- Test statistic of the generalized likelihood ratio:

$$
\Lambda=\frac{\max_{\theta\in\omega_0}[lik(\theta)]}{\max_{\theta\in\Omega}[lik(\theta)]}
$$

- Under **smoothness** conditions on the probability density or frequency functions involved, the null distribution of $-2\log\Lambda$ tends to a chi-square distribution with degrees of freedom equal to $\dim\Omega-\dim\omega_0$ as the **sample size tends to infinity**



## Likelihood Ratio Tests for the Multinomial Distribution

- $O_i=n\widehat{p}_i$, $E_i=np_i(\widehat{\theta})$
- Likelihood ratio test statistic:

$$
-2\log\Lambda=-2n\sum^{m}_{i=1}\widehat{p}_i\log(\frac{p_i(\widehat{\theta})}{\widehat{p}_i})=2\sum^{m}_{i=1}O_i\log(\frac{O_i}{E_i})
$$

- Pearson's chi-square statistic:（卡方分布的自由度使用上面的规则）

$$
X^2=\sum^{m}_{i=1}\frac{[x_i-np_i(\widehat{\theta})]^2}{np_i(\widehat{\theta})}=\sum^{m}_{i=1}\frac{(O_i-E_i)^2}{E_i}
$$



## The Poisson Dispersion Test

- 原假设：计数服从参数相同的泊松分布；备择假设：计数服从参数不同的泊松分布
- Likelihood ratio test statistic:

$$
-2\log\Lambda=2\sum^{n}_{i=1}x_i\log(\frac{x_i}{\overline{x}})
$$

- Poisson Dispersion Test:

$$
-2\log\Lambda\approx\frac{n\widehat{\sigma}^2}{\overline{x}}
$$



## Hanging Rootograms

- Hanging Histogram: $n_j-\widehat{n}_j$
  - 较大的偏差可能说明拟合模型不合适，也可能仅仅来自于较大的随机变异性

- Variance-Stabilizing Transformation: Suppose that a random variable $X$ has mean $\mu$ and variance $\sigma^2(\mu)$, which depends on $\mu$. If $Y=f(X)$, $f$ is chosen so that $\sigma^2(\mu)[f'(\mu)]^2$ is constant, the variance of $Y$ will not depend on $\mu$.
  - $E(\widehat{\theta})=\mu$, $Var(\widehat{\theta})=\sigma^2(\mu)$
- Hanging Rootogram: $\sqrt{n_j}-\sqrt{\widehat{n}_j}$
  - $\sigma^2(\mu)[f'(\mu)]^2=c$
  - 单元与单元之间的偏差具有近似相同的统计变异性
- Hanging Chi-gram: $\frac{n_j-\widehat{n}_j}{\sqrt{\widehat{n}_j}}$
  - $Var(\frac{n_j-\widehat{n}_j}{\sqrt{\widehat{n}_j}})=\frac{Var(n_j)}{\widehat{n}_j}=\frac{np_j(1-p_j)}{\widehat{n}_j}\approx\frac{np_j}{\widehat{n}_j}=1$



## Probability Plot

![image-20220108212516985](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220108212516985.png)
