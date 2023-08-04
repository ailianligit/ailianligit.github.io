# Summarizing Data

## Methods Based on the Cumulative Distribution Function

### The Empirical Cumulative Distribution Function

- **ECDF**: $F_n(x)=\frac{1}{n}(\#x_i\le x)$ (unbiased)



### The Survival Function

- **Survival Function**: $S(t)=P(T>t)=1-F(t)$
- **Empirical Survival Function**: $S_n(t)=1-F_n(t)$
- **Hazard Function**: $h(t)=\frac{f(t)}{1-F(t)}=-\frac{d}{dt}\log S(t)$
  - 指数分布的$h(t)=\lambda$，无记忆性



### Two-Sample Empirical Quantile-Quantile Plots

- $y_p=x_p+h$, slope = 1, intercept = h
- $y_p=cx_p$, slope = c, intercept = 0



## Histograms, Density Curves, and Stem-and-Leaf Plots

- Kernel probability density estimate:
  $$
  f_h(x)=\frac{1}{n}\sum^n_{i=1}w_h(x-X_i)=\frac{1}{nh}\sum^n_{i=1}w(\frac{x-x_i}{h})
  $$

![image-20220108224856372](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079613.png)



## Measures of Location

### The Arithmetic Mean

$$
\overline{x}=\frac{1}{n}\sum^{n}_{i=1}x_i
$$



### The Median (Continuous)

$$
P(X_i>\eta)=P(X_i<\eta)=\frac{1}{2}
\\P(X_{(m)}\le\eta\le X_{(k)})=1-P(\eta< X_{(m)})-P(\eta>X_{(k)})
\\P(\eta<X_{(m)})=\sum^{m-1}_{j=0}P(j\text{ observation are less than }\eta)=\sum^{m-1}_{j=0}\binom{n}{j}
\\P(\eta>X_{(k)})=\sum^{n-k}_{j=0}P(j\text{ observation are bigger than }\eta)=\sum^{n-k}_{j=0}\binom{n}{j}
$$

$$
P(X_{(k)}\le\eta\le X_{(n-k+1)})=1-\frac{1}{2^{n-1}}\sum^{k-1}_{j=0}\left(\begin{matrix}n\\j\end{matrix}\right)=1-2P(Y\le k-1)
$$



### The Trimmed Mean

- $\overline{x}_\alpha=\frac{x_{([n\alpha]+1)}+\cdots+x_{(n-[n\alpha])}}{n-2[n\alpha]}$



### M Estimates

- Least Squares Estimate (Mean): $\min\sum^n_{i=1}(\frac{X_i-\mu}{\sigma})^2$
- Median is the minimizer of  $\min\sum^n_{i=1}\abs{\frac{X_i-\mu}{\sigma}}$
- M Estimates: $\min\sum^n_{i=1}\Phi(\frac{X_i-\mu}{\sigma})$
  - $k=\infty$对应均值，$k=0$对应中位数，通常选择$k=1.5$



## Measures of Dispersion

- Interquartile Range: $\text{IQR}=q_{0.75}-q_{0.25}$
- Median Absolute Deviation from the Median (MAD): $\text{MAD}=\text{median}(\abs{x_i-\text{median}(x_1,\cdots,x_n)})=\min\frac{1}{n}\sum^n_{i=1}\abs{X_i-\overline{X}}$
- 正态分布的散度度量：$s$，$\frac{\text{IQR}}{1.35}$，$\frac{\text{MAD}}{0.675}$



## Boxplots

![image-20220108232412287](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079609.png)
