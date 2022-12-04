# Random Variables

## Discrete Random Variables

### Bernoulli Random Variables

- 频率函数（概率质量函数）：$p(1)=p,p(0)=1-p$
- 期望：$E(X)=p$
- 方差：$Var(X)=p(1-p)$

### The Binomial Distribution

- 频率函数：$P(X=k)=\tbinom{n}{k}p^k(1-p)^{n-k}$
- 期望：$E(X)=np$
- 方差：$Var(X)=np(1-p)$
- 若$X_1,\dots,X_n$是独立的伯努利随机变量，则$Y=\sum^n_{i=1}X_i$是二项随机变量

### The Geometric Distribution

- 频率函数：$P(X=k)=(1-p)^{k-1}p$
- 期望：$E(X)=\frac{1}{p}$
- 方差：$Var(X)=\frac{1-p}{p^2}$
- 特点：无记忆性

### The Negative Binomial Distribution

- 频率函数：$P(X=k)=\tbinom{k-1}{r-1}p^r(1-p)^{k-r}$
- 期望：$E(X)=r\frac{1}{p}$
- 方差：$Var(X)=r\frac{1-p}{p^2}$
- 若$X_1,\dots,X_n$是独立的几何随机变量，则$Y=\sum^n_{i=1}X_i$是负二项随机变量

### The Hypergeometric Distribution

- 频率函数：$P(X=k)=\frac{\tbinom{r}{k}\tbinom{n-r}{m-k}}{\tbinom{n}{m}}$
- 期望：$E(X)=r\frac{m}{n}$

### The Poisson Distribution

- 频率函数：$P(X=k)=\frac{\lambda^k}{k!}e^{-\lambda},\lambda>0$
- 期望：$E(X)=\lambda$
- 方差：$Var(X)=\lambda$
- 若$n$很大，$p$很小，可以用泊松分布近似二项分布，$\lambda=np$
- 参数为$\lambda$的独立泊松分布的和是参数为$n\lambda$的泊松分布
- 理解：事件在给定时间区间内发生的次数



## Continuous Random Variables

### Uniform Random Variables

- 密度函数：$f(x)=1/(b-a),a\le x\le b$
- 期望：$E(X)=\frac{a+b}{2}$
- 方差：$Var(X)=\frac{1}{12}(b-a)^2$

### The Exponential Density

- 密度函数：$f(x)=\lambda e^{-\lambda x},x\ge0$
- 期望：$E(X)=\frac{1}{\lambda}$
- 方差：$E(X)=\frac{1}{\lambda^2}$
- 特点：无记忆性
- 理解：1个事件发生需要的时间

### The Gamma Density

- 伽马函数：$\Gamma(\alpha)=\int^{+\infty}_0t^{\alpha-1}e^{-t}dt$
- 密度函数：$f(x)=\frac{\lambda^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-\lambda x},x\ge0$
- 期望：$E(X)=\frac{\alpha}{\lambda}$
- 方差：$Var(X)=\frac{\alpha}{\lambda^2}$
- 若$X_1,\dots,X_n$服从独立的参数为$\lambda_0$指数分布，则$Y=\sum^n_{i=1}X_i$服从参数为$\alpha=n$，$\lambda=\lambda_0$的伽马分布
  - 参数$\alpha=1$伽马分布与指数分布相同
- 标准正态分布的平方服从参数为$\alpha=\frac{1}{2}$和$\lambda=\frac{1}{2}$的伽马分布
  - 证明方法：求解标准正态分布的平方的密度函数
- 自由度为1的卡方分布服从参数为$\alpha=\frac{1}{2}$和$\lambda=\frac{1}{2}$的伽马分布
- 理解：$\alpha$个事件发生需要的时间

### The Normal Distribution

- 密度函数：$f(x)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
- 标准正态分布的平方服从参数为$\alpha=\frac{1}{2}$和$\lambda=\frac{1}{2}$的伽马分布
  - 证明方法：求解标准正态分布的平方的密度函数
- 标准正态分布的平方服从自由度为1的卡方分布
- 独立标准正态分布的比值服从标准柯西分布
  - 证明方法：求解两个独立标准正态分布的比值的密度函数
- 独立标准正态分布的比值服从自由度为1的t分布

### The Beta Density

- 密度函数：$f(x)=\frac{1}{B(a,b)}x^{a-1}(1-x)^{b-1}=\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}x^{a-1}(1-x)^{b-1},0\le x\le 1$
- 期望：$E(X)=\frac{a}{a+b}$
- 参数$a=1$和$b=1$的贝塔分布服从均匀分布

### The Standard Cauchy Distribution

- 密度函数：$f(x)=\frac{1}{\pi(x^2+1)}$
- 特点：**期望和方差无定义**
- 标准柯西分布是两个独立标准正态分布的比值
  - 证明方法：求解两个独立标准正态分布的比值的密度函数
- 自由度为1的t分布是标准柯西分布
  - 证明方法：计算两个独立标准正态分布的比值的密度函数，证明其为标准柯西分布，自由度为1的t分布是两个独立标准正态分布的比值



## Functions of a Random Variable

- $P(Z\le z)=P(F(X)\le z)=P(X\le F^{-1}(z))=F(F^{-1}(z))=z$, $Z\sim U(0,1)$
- U is uniform on [0, 1], $P(X\le x)=P(F^{-1}(U)\le x)=P(U\le F(x))=F(x)$