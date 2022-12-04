## Population

#### Size: $$N$$

#### Value of Member: $$x_i$$ (fixed)

#### Mean: $$\mu=\frac{1}{N}\sum_{i=1}^Nx_i$$

#### (Dichotomous Case) Proportion: $$p=\mu$$

#### Total: $$\tau=\sum_{i=1}^Nx_i=N\mu$$

#### Variance: $$\sigma^2=\frac{1}{N}\sum_{i=1}^N(x_i-\mu)^2=\frac{1}{N}\sum_{i=1}^Nx_i^2-\mu^2$$

- (dichotomous case) $$\sigma^2=p(1-p)$$

#### Standard Deviation: $\sigma$



## Sample

#### Size: $$n$$

- Sampling Fraction: $\frac{n}{N}$

#### Value of Member: $$X_i$$​ (random)

- Mean: $$E(X_i)=\mu$$
- Variance: $$Var(X_i)=\sigma^2$$

#### Estimate of Population Mean: $$\overline{X}=\frac{1}{n}\sum_{i=1}^nX_i$$

- Mean: $$E(\overline{X})=\mu$$​​​ (unbiased)
- Variance: $Var(\overline{X})=\frac{\sigma^2}{n}(\frac{N-n}{N-1})=\frac{\sigma^2}{n}(1-\frac{n-1}{N-1})$
  - Finite Population Correction: $(1-\frac{n-1}{N-1})$（不考虑有限总体校正的情况有i.i.d和重复抽样）
  - (Replacement or Independent) Variance: $Var(\overline{X})=\frac{\sigma^2}{n}$
  - Unbiased Estimate of $Var(\overline{X})$ (Estimated Standard Error): $s_{\overline{X}}^2=\frac{(Unbiased\,Estimate\,of\,\sigma^2)}{n}(\frac{N-n}{N-1})=\frac{s^2}{n}(1-\frac{n}{N})$
  - $s^2=\frac{1}{n-1}\sum_{i=1}^{n}(X_i-\overline{X})^2=\frac{n}{n-1}\widehat{p}(1-\widehat{p})$​ (dichotomous case)
- Standard Deviation (Standard Error): $\sigma_{\overline{X}}$
  - (Replacement or Independent)  $\sigma_{\overline{X}}=\frac{\sigma}{\sqrt{n}}$

#### (Dichotomous Case) Estimate of Population Proportion: $\widehat{p}$

- Mean: $E(\widehat{p})=p$ (unbiased)
- Variance: $Var(\widehat{p})=\frac{p(1-p)}{n}(\frac{N-n}{N-1})$
  - Unbiased Estimate of $Var(\widehat{p})$ (Estimated Standard Error): $s_{\widehat{p}}^2=\frac{\widehat{p}(1-\widehat{p})}{n-1}(1-\frac{n}{N})$

#### Estimate of Population Total: $T=N\overline{X}$

- Mean: $$E(T)=\tau$$​ (unbiased)
- Variance: $Var(T)=N^2(\frac{\sigma^2}{n})\frac{N-n}{N-1}$
  - Unbiased Estimate of $Var(T)$ (Estimated Standard Error): $s_T^2=N^2s_{\overline{X}}^2$

#### Estimate of Population Variance: $\widehat{\sigma}^2=\frac{1}{n}\sum_{i=1}^{n}(X_i-\overline{X})^2$

- Mean: $E(\widehat{\sigma}^2)=\sigma^2(\frac{n-1}{n})\frac{N}{N-1}$
- Unbiased Estimate of $\sigma^2$: $(\frac{n}{n-1})\frac{N-1}{N}\widehat{\sigma}^2=\frac{N-1}{N(n-1)}\sum_{i=1}^{n}(X_i-\overline{X})^2$
  - (Replacement or Independent) $E(s^2)=\sigma^2$



## Estimation of Ratio

- Ratio: $r=\frac{\sum^{N}_{i=1}y_i}{\sum^{N}_{i=1}x_i}=\frac{\mu_y}{\mu_x}$
  - Estimate of $r$: $R=\frac{\overline{Y}}{\overline{X}}$
- Population Covariance: $\sigma_{xy}=\frac{1}{N}\sum^N_{i=1}(x_i-\mu_x)(y_i-\mu_y)$
  - $\text{Cov}(\overline{X},\overline{Y})=\frac{\sigma_{xy}}{n}(\frac{N-n}{N-1})$
  - Estimate: $s_{xy}=\frac{1}{n-1}\sum^n_{i=1}(X_i-\overline{X})(Y_i-\overline{Y})=\frac{1}{n-1}(\sum_{i=1}^nX_iY_i-n\overline{X}\cdot\overline{Y})$
- Population Correlation Coefficient: $\rho=\frac{\sigma_{xy}}{\sigma_x\sigma_y}$
  - Estimate: $\widehat{\rho}=\frac{s_{xy}}{s_x s_y}$
- Ratio Estimate of $\mu_y$: $\overline{Y}_R=\frac{\mu_x}{\overline{X}}\overline{Y}=\mu_x R$
  - Coefficients of Variation: $C_x=\frac{\sigma_x}{\mu_x}$, $C_y=\frac{\sigma_y}{\mu_y}$
  - 在忽略有限总体校正的情况下，当$\rho>\frac{1}{2}(\frac{C_x}{C_y})$时，比率估计$Y_R$相比普通估计$\overline{Y}$有更小的标准差



## Stratified Random Sampling

- 假设不同层的样本是独立的
- 总体比例：$W_l=\frac{N_l}{N}$
- 总体均值：$\mu=\sum^L_{l=1}W_l\mu_l$
  - 第$l$层的样本均值：$\overline{X}_l=\frac{1}{n_l}\sum^{n_l}_{i=1}X_{il}$，$E(\overline{X}_l)=\mu_l$
  - 忽略有限总体校正，$\text{Var}(\overline{X}_{l})=\frac{\sigma_l^2}{n_l}$
  - 总体均值的无偏估计：$\overline{X}_s=\sum^L_{l=1}W_l\overline{X}_l$，$E(\overline{X}_s)=\mu$
  - 忽略有限总体校正，$\text{Var}(\overline{X}_{sp})=\frac{1}{n}\sum^L_{l=1}W_l\sigma^2_l$
- 总体总数的分层估计是无偏估计：$T_s=N\overline{X}_s$，$E(T_s)=\tau$
- 奈曼分配：$n_l=n\frac{W_l\sigma_l}{\sum^L_{k=1}W_k\sigma_k}$
  - 限制条件：$n_1+\cdots+n_L=n$，最小化$Var(\overline{X}_s)$的样本容量
  - 忽略有限总体校正，$\text{Var}(\overline{X}_{so})=\frac{1}{n}(\sum^L_{l=1}W_l\sigma_l)^2$
  - $Var(\overline{X}_{sp})-Var(\overline{X}_{so})=\frac{1}{n}\sum^L_{i=1}W_l(\sigma_l-\overline{\sigma})^2$，$\overline{\sigma}=\sum^L_{l=1}W_l\sigma_l$，在**忽略有限总体校正**的情况下，如果各层的**方差**相同，比例分配得到的结果与奈曼分配得到的结果相同，方差越大，奈曼分配越好
- 比例分配：$n_l=nW_l$
  - $Var(\overline{X})-Var(\overline{X}_{sp})=\frac{1}{n}\sum^L_{i=1}W_l(\mu_l-\mu)^2$，在**忽略有限总体校正**的情况下，如果层**均值**有变异，那么比例分配的分层随机抽样优于简单随机抽样



## Independent Normal Random Variables

#### Sample Mean: $\overline{X}=\frac{1}{n}\sum_{i=1}^{n}X_i$

- $\overline{X}\sim N(\mu,\frac{\sigma^2}{n})$

#### Sample Variance: $S^2=\frac{1}{n-1}\sum_{i=1}^{n}(X_i-\overline{X})^2$

- $\frac{(n-1)S^2}{\sigma^2}\sim\chi_{n-1}^2$
- $\frac{\overline{X}-\mu}{S/\sqrt{n}}\sim t_{n-1}$



## Measurement Error

#### True Value: $x_0$

#### Bias (Systematic Error): $\beta$

#### Random Error: $\varepsilon$

- Mean: $E(\varepsilon)=0$
- Variance: $Var(\varepsilon)=\sigma^2$

#### Measurement: $X=x_0+\beta+\varepsilon$​

- Mean: $E(X)=x_0+\beta$
- Variance: $Var(X)=\sigma^2$

#### Mean Squared Error: $MSE=E[(X-x_0)^2]=\beta^2+\sigma^2$



