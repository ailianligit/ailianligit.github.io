《数理统计》期中Project

18324030 李洋 19信计



An operator counted the number of fibers in each of 23 grid squares, yielding the following counts (Steel et al. 1980):

31, 29, 19, 18, 31, 28, 34, 27, 34, 30, 16, 18, 26, 27, 27, 18, 24, 22, 28, 24, 21, 17, 24

The poisson distribution would be a plausible model for describing the variability from grid square to grid square in the situation and could be used to characterize the inherent variability in future measurements.



1.Parameter estimation of $\lambda$ using MOM & MLE.

泊松分布的一阶矩：
$$
\widehat{\mu}_1=\frac{1}{n}\sum_{i=1}^{n}X_i=\frac{1}{23}\sum_{i=1}^{23}X_i=24.9130
$$
因此，$\lambda$的矩估计：
$$
\widehat{\lambda}=\overline{X}=\widehat{\mu}_1=24.9130
$$
对数似然：
$$
l(\lambda)=\log\lambda\sum_{i=1}^{n}X_i-n\lambda-\sum_{i=1}^{n}\log X_i!
$$
令对数似然的一阶导数为0：
$$
l'(\lambda)=\frac{1}\lambda\sum_{i=1}^{n}X_i-n=0
$$
因此，$\lambda$的最大似然估计：
$$
\widehat{\lambda}=\overline{X}=24.9130
$$
MOM和MLE估计$\lambda$拟合的泊松分布的频率函数：
$$
P(X=x)=\frac{24.91^xe^{-24.91}}{x!}
$$
样本数据的直方图、拟合的泊松分布的频率函数曲线图：

（因为频率函数值是频率，因此将每个频率函数值乘样本数$n$，以便将直方图与频率函数曲线图放在一起比较）

<img src="D:\OneDrive - 中山大学\2021-1\数理统计\Project\MOM_MLE.bmp" alt="MOM_MLE" style="zoom: 50%;" />

$X_i$服从独立的泊松分布，因此$S=\sum X_i$也服从泊松分布。当$n\widehat{\lambda}$很大时，$S$的分布近似正态，因此$\widehat{\lambda}=S/n$也是近似正态的。

$\widehat{\lambda}$的估计标准误差：
$$
s_{\widehat{\lambda}}=\sqrt{\frac{\widehat{\lambda}}{n}}=\sqrt{\frac{24.91}{23}}=1.0408
$$
查询标准正态分布表，知$z(0.05)=1.65$。

因此，$\lambda$的近似90％置信区间：
$$
\widehat{\lambda}\pm1.65s_{\widehat{\lambda}}或(23.19,26.63)
$$



2.Parameter estimation of $\lambda$ using Bayesian Approach. Use gamma distribution distribution as priors.

给定$\lambda$，$X_i$服从独立的泊松分布，因此联合分布是各自边际分布的乘积。

因此，似然：
$$
f_{X\mid\Lambda}(x|\lambda)=\frac{\lambda^{\sum_{i=1}^{n}x_i}e^{-n\lambda}}{\prod_{i=1}^{n}x_i!}
$$
假设先验密度为$f_\Lambda(\lambda)$，则后验密度：
$$
f_{\Lambda\mid X}(\lambda|x)=\frac{\lambda^{\sum_{i=1}^{n}x_i}e^{-n\lambda}f_\Lambda(\lambda)}{\int\lambda^{\sum_{i=1}^{n}x_i}e^{-n\lambda}f_\Lambda(\lambda)d\lambda}
$$
假设先验密度$f_\Lambda(\lambda)$是伽马密度，使用矩估计方法估计伽马密度的参数$\alpha$和$\upsilon$：
$$
\alpha=\frac{\mu^2}{\sigma^2}=20.6316
$$

$$
\upsilon=\frac{\mu}{\sigma^2}=\frac{24.9130}{30.0830}=0.8281
$$

因此，先验密度：
$$
f(\lambda)=\frac{\lambda^{\alpha-1}\upsilon^\alpha e^{-\upsilon\lambda}}{\Gamma(\alpha)}=\frac{\lambda^{19.63}\cdot0.83^{20.63}\cdot e^{-0.83\lambda}}{8.02\times10^{17}},\lambda>0
$$
后验密度：
$$
f_{\Lambda\mid X}(\lambda|x)
=\frac{\lambda^{\sum x_i+\alpha-1}e^{-(n+\upsilon)\lambda}}{\int_0^\infty\lambda^{\sum x_i+\alpha-1}e^{-(n+\upsilon)\lambda}d\lambda}
=\frac{\lambda^{\sum x_i+\alpha-1}\upsilon^{\sum x_i+\alpha}e^{-(n+\upsilon)\lambda}}{\Gamma(\sum x_i+\alpha)}
\sim\Gamma(593.63,23.83)
$$
<img src="D:\OneDrive - 中山大学\2021-1\数理统计\Project\Bayesian1.bmp" alt="Bayesian1" style="zoom:50%;" />

后验密度具有伽马密度的形式，其中两个参数分别是${\sum x_i+\alpha}$和${n+\upsilon}$。

因此，选取后验均值作为以伽马分布为先验分布的贝叶斯参数估计：
$$
\mu_{post}=\frac{\sum x_i+\alpha}{n+\upsilon}=24.9130
$$
以伽马分布为先验分布的贝叶斯参数估计拟合的泊松分布的频率函数：
$$
P(X=x)=\frac{24.91^xe^{-24.91}}{x!}
$$
选取后验分布的标准差作为变异性的度量：
$$
\sigma_{post}=\frac{\sqrt{\sum x_i+\alpha}}{n+\upsilon}=1.0225
$$
因此，$\lambda$的近似90％置信区间：
$$
\mu_{post}\pm1.65\sigma_{post}或(23.23,26.60)
$$



3.Parameter estimation of $\lambda$ using Bayesian Approach. Use uniform distribution distribution as priors.

假设先验密度$f_\Lambda(\lambda)$是均匀密度：
$$
f(\lambda)=\begin{cases}\frac{1}{100}=0.01,\quad 0\leq\lambda\leq 100\\0,\quad otherwise\end{cases}
$$
选择$a$尽量小，$b$尽量大的均匀密度可以方便后验密度的计算。若先验满足$a$尽量小，$b$尽量大，则后验密度分母上的定积分可以视为对参数为$\sum x_i+1$和$n$的伽马分布作积分，其结果使得包括分子和分母在内的整个后验密度就是参数为$\sum x_i+1$和$n$的伽马分布。

后验密度：
$$
f_{\Lambda\mid X}(\lambda|x)
=\frac{\frac{1}{100}\lambda^{\sum x_i}e^{-n\lambda}}{\frac{1}{100}\int_0^{100}\lambda^{\sum x_i}e^{-n\lambda}d\lambda}
\sim\Gamma(574,23)
$$
<img src="D:\OneDrive - 中山大学\2021-1\数理统计\Project\Bayesian2.bmp" alt="Bayesian2" style="zoom:50%;" />

选取后验均值作为以均匀分布为先验分布的贝叶斯参数估计：
$$
\mu_{post}=\frac{\sum x_i+1}{n}=24.9565
$$
以均匀分布为先验分布的贝叶斯参数估计拟合的泊松分布的频率函数：
$$
P(X=x)=\frac{24.96^xe^{-24.96}}{x!}
$$
选取后验分布的标准差作为变异性的度量：
$$
\sigma_{post}=\frac{\sqrt{\sum x_i+1}}{n}=1.0417
$$
因此，$\lambda$的近似90％置信区间：
$$
\mu_{post}\pm1.65\sigma_{post}或(23.24,26.68)
$$

```matlab
clear;clc
%% 样本数据的直方图
data=[31,29,19,18,31,28,34,27,34,30,16,18,26,27,27,18,24,22,28,24,21,17,24];
histogram(data);
hold on;

%% 矩估计和最大似然估计
x=min(data):1:max(data);
lambda=mean(data);          		%泊松分布的估计参数
sigma=sqrt(lambda/size(data,2));    %用于计算置信区间的平方差
y=power(exp(1),-lambda)*power(lambda,x)./factorial(x);
plot(x,y*size(data,2),'k',x,y*size(data,2),'ko');
xlabel('Number of Fibers');
ylabel('Counts');
```

```matlab
clear;clc
%%
data=[31,29,19,18,31,28,34,27,34,30,16,18,26,27,27,18,24,22,28,24,21,17,24];
x=10:1:35;

%% 以伽马分布为先验（矩估计确定伽马分布的参数）
palpha=mean(data)^2/var(data);
plambda=mean(data)/var(data);
p=gampdf(x,palpha,1/plambda);
plot(x,p);
%xlabel('\lambda');
hold on;

%% 贝叶斯估计过程（后验分布也是伽马分布）
qalpha=sum(data)+palpha;
qlambda=size(data,2)+plambda;
q=gampdf(x,qalpha,1/qlambda);
plot(x,q,'--');
legend('prior','posterior');
hold on;
qmu=qalpha/qlambda;             %泊松分布的估计参数
qsigma=sqrt(qalpha)/qlambda;    %用于计算置信区间的平方差

%% 以均匀分布为先验（区间[0,100]上的均匀分布）
r=zeros(1,size(x,2));
r(:)=0.01;
plot(x,r,'-.');
xlabel('\lambda');
hold on;

%% 贝叶斯估计过程（后验分布也是伽马分布）
salpha=sum(data)+1;
slambda=size(data,2);
s=gampdf(x,salpha,1/slambda);
plot(x,s);
legend('prior1','posterior1','prior2','posterior2');
smu=salpha/slambda;             %泊松分布的估计参数
ssigma=sqrt(salpha)/slambda;    %用于计算置信区间的平方差
```

<img src="D:\OneDrive - 中山大学\2021-1\数理统计\Project\Bayesian.bmp" alt="Bayesian2" style="zoom:50%;" />
