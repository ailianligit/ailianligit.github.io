# Comparing Two Samples

## Comparing Two Independent Samples

### Methods Based on the Normal Distribution

- The case of $\sigma$ is known: Normal distribution

  - MLE of $\mu_X-\mu_Y$ is $\overline{X}-\overline{Y}$

  - $\overline{X}-\overline{Y}\sim N(\mu_X-\mu_Y,\sigma^2(\frac{1}{n}+\frac{1}{m}))$
- The case of $\sigma$ is unknown: t distribution
  - $s_X^2=\frac{1}{n-1}\sum^n_{i=1}(X_i-\overline{X})^2$，$s_Y^2=\frac{1}{m-1}\sum^m_{i=1}(Y_i-\overline{Y})^2$
  - $s_p^2=\frac{(n-1)s_X^2+(m-1)s_Y^2}{m+n-2}$，$s_{\overline{X}-\overline{Y}}=s_p\sqrt{\frac{1}{n}+\frac{1}{m}}$
  - $t=\frac{(\overline{X}-\overline{Y})-(\mu_X-\mu_Y)}{s_{\overline{X}-\overline{Y}}}$
  - 假设检验的检验统计量：$t=\frac{\overline{X}-\overline{Y}}{s_{\overline{X}-\overline{Y}}}$
    - $H_1:\mu_X\ne\mu_Y$,$\abs{t}>t_{n+m-2}(\alpha/2)$
    - $H_2:\mu_X>\mu_Y$,$t>t_{n+m-2}(\alpha)$
    - $H_3:\mu_X<\mu_Y$,$t<-t_{n+m-2}(\alpha)$



### A Nonparametric Method - The Mann-Witney Test

- 秩和检验
- 小秩太小或大秩太大拒绝原假设



## Comparing Paired Samples

### Methods Based on the Normal Distribution

- The case of $\sigma_D$ is unknown: t distribution
  - $\overline{D}=\overline{X}-\overline{Y}$
  - $E(D_i)=\mu_X-\mu_Y=\mu_D$, $Var(D_i)=\sigma^2_X+\sigma^2_Y+2\sigma_{XY}=\sigma^2_D$
  - $s_D^2=\frac{1}{n-1}\sum^n_{i=1}(D_i-\overline{D})^2$
  - $t=\frac{\overline{D}-\mu_D}{s_{\overline{D}}}$
- 相对效率：$\frac{Var(\overline{D})}{Var(\overline{X}-\overline{Y})}$



### A Nonparametric Method - The Signed Rank Test

- The Signed Rank Test
- 不依赖于正态性假设，对离群值不敏感
- 单边符号秩不能太小



## Parameters Estimating & Hypotheses Testing of Normal Distribution 

- 独立同分布或重复抽样的前提下，不考虑有限总体校正
- 已知$\sigma^2$情况下，$\sigma_{\overline{X}}=\frac{\sigma}{\sqrt{n}}$；未知$\sigma^2$情况下，$s_{\overline{X}}=\frac{s}{\sqrt{n}}=\frac{\sum^{n}_{i=1}X_i}{\sqrt{n(n-1)}}$
- 6.3：未知$\sigma^2$情况下统计量$\frac{\overline{X}-\mu}{s_{\overline{X}}}\sim t_{n-1}$，未知$\mu$情况下统计量$\frac{(n-1)S^2}{\sigma^2}\sim\chi^2_{n-1}$
- 8.5B：对$\mu$的最大似然估计为$\overline{X}$，$\sigma^2$的最大似然估计为$\frac{1}{n}\sum^{n}_{i=1}(X_i-\overline{X})^2$
- 8.5.3A：对$\mu$和$\sigma^2$最大似然估计的置信区间（使用6.3的统计量）
- 9.2A：已知$\sigma^2$情况下，简单假设的似然比检验（检验统计量$\frac{\overline{X}-\mu_0}{\sigma_{\overline{X}}}\sim N(0,1)$）
- 9.3A：已知$\sigma^2$情况下，原假设参数$\mu$的置信区间（统计量$\frac{\overline{X}-\mu_0}{\sigma_{\overline{X}}}\sim N(0,1)$）
- 9.4A：已知$\sigma^2$情况下，复杂假设的广义似然比检验（检验统计量$\frac{\overline{X}-\mu_0}{\sigma_{\overline{X}}}\sim N(0,1)$）
- 11.2.1：已知$\sigma^2$情况下，$\frac{(\overline{X}-\overline{Y})-(\mu_X-\mu_Y)}{\sigma_{\overline{X}-\overline{Y}}}\sim N(0,1)$；未知$\sigma^2$情况下，$\frac{(\overline{X}-\overline{Y})-(\mu_X-\mu_Y)}{s_{\overline{X}-\overline{Y}}}\sim t_{m+n-2}$
- 11.2.1：两样本比较中，假设统计量检验与似然比检验的等价性
- EX：11E14 11E19b