# Distributed Policy Gradient for Federated Reinforcement Learning

## Abstract



## 1 Introduction

强化学习在决策问题上发挥着重要的作用，然而，由于采样效率低，强化学习仍然难以在现实世界中广泛应用。

例如，当我们需要对某一种疾病做临床决策时，每个医院能够采集的样本数少，并且需要对样本的隐私进行保护，传统的强化学习难以在这样的场景下应用。

联邦学习的优势在于对隐私的保护和训练效果的提升。客户机（client）只需要向服务器（server）交换梯度，而不需要上传原始数据。因此将联邦学习和强化学习相结合是一个自然的想法。

然而，中心化的联邦学习（Centralized FRL）容易受到通信效率的限制。对于大规模的智能体网络，在服务器和客户机之间交换梯度和更新模型可能会带来巨额的时间开销，因此我们需去中心化的联邦强化学习（Decentralized FRL）来降低通信的成本。

在现实场景中，联邦学习容易受到随机故障（random failures）或对抗性攻击（adversarial attacks）的影响。拜占庭故障模型（Byzantine failure model）是分布式计算中最严格的故障形式。

**Novelty.** 我们基于Fault-Tolerant Federated Reinforcement Learning with Theoretical中将联邦学习、策略梯度（policy gradient）和拜占庭故障模型相结合的方式，提出了去中心场景下的具备拜占庭弹性的联邦强化学习框架。



## 2 Centralized FRL with byzantine resilience



## 3



### 2.1 Federated learning setting

在中心化的联邦学习设置中，中心服务器被认为是可以信赖的（trustworthy），$K$个分布式智能体表示为$k\in\{1,\dots,K\}$。在每一轮$t\in\{1,\dots,T\}$，中心服务器将策略网络的参数$\boldsymbol{\theta}_0^t$广播给所有的智能体。每个智能体使用获得的策略与独立地与环境交互，采集一批（batch）轨迹（trajectories）$\{\tau_{t,i}^{(k)}\}_{i=1}^{B_t}\sim p(\cdot\mid\boldsymbol{\theta}_0^t)$。智能体根据本地轨迹计算的梯度$\mu_{t}^{(k)}$会直接发送给服务器。

服务器利用拜占庭过滤器对所有智能体的梯度进行聚合，然后做策略梯度上升，得到新的策略网络的参数。为了考虑潜在的随机故障和对抗性攻击，我们允许$\alpha\in[0,0.5)$的智能体是拜占庭节点。每一轮不超过一半的智能体会向服务器发送任意的向量，服务器只能访问从$K$个智能体接收到的$K$个梯度，而不知道哪些智能体是拜占庭节点。



### 2.2 Policy gradient estimator

策略梯度方法的目标是最大化期望回报$\mathbb{E}_{\tau \sim p(\cdot \mid \boldsymbol{\theta})}[\mathcal{R}(\tau)]$，其策略梯度表示为
$$
\nabla_{\boldsymbol{\theta}} J(\boldsymbol{\theta})=\int_{\tau} \mathcal{R}(\tau) \nabla_{\boldsymbol{\theta}} p(\tau \mid \boldsymbol{\theta}) \mathrm{d} \tau=\mathbb{E}_{\tau \sim p(\cdot \mid \boldsymbol{\theta})}\left[\nabla_{\boldsymbol{\theta}} \log p(\tau \mid \boldsymbol{\theta}) \mathcal{R}(\tau)\right]
$$
对于REINFORCE或GPOMDP策略梯度方法，策略梯度的估计表示为
$$
\widehat{\nabla}_{B_t}^{(k)} J(\boldsymbol{\boldsymbol{\theta}_{0}^{t}})=\frac{1}{B_t} \sum_{i=1}^{B_t} g\left(\tau_{t,i}^{(k)} \mid \boldsymbol{{\theta}_{0}^{t}}\right)
$$
其中$g\left(\tau \mid \boldsymbol{{\theta}_{0}^{}}\right)$是$\nabla_{\boldsymbol{{\theta}_{0}^{}}} \log p(\tau \mid \boldsymbol{{\theta}_{0}^{}}) \mathcal{R}(\tau)$的无偏估计。智能体发送给服务器的梯度表示为$\mu_{t}^{(k)} \triangleq \widehat{\nabla}_{B_t}^{(k)} J(\boldsymbol{\boldsymbol{\theta}_{0}^{t}})$。

### 3.1 Problem statement

**Algorithm 1 FedPG-Cen**

1. **Input:** $\tilde{\boldsymbol{\theta}}_{0} \in \mathbb{R}^{d}$, batch size $B_t$, mini batch size $b_t$, step size $\eta_t$
2. for $t=1$ to $T$ do
3. ​    $\boldsymbol{\theta}_{0}^{t} \leftarrow \tilde{\boldsymbol{\theta}}_{t-1}$
4. ​    for $k=1$ to $K$ do
5. ​        Sample $B_t$ trajectories $\left\{\tau_{t, i}^{(k)}\right\}_{i=1}^{B_{t}}$ from $p\left(\cdot \mid \boldsymbol{\theta}_{0}^{t}\right)$
6. ​        $\mu_{t}^{(k)} \triangleq\frac{1}{B_{t}} \sum_{i=1}^{B_{t}} g\left(\tau_{t, i}^{(k)} \mid \boldsymbol{\theta}_{0}^{t}\right)$
7. ​    $\mu_{t}=\frac{1}{K}\sum^{K}_{k=1}\mu_{t}^{(k)}$
8. ​    Sample $N_{t} \sim \operatorname{Geom}\left(\frac{B_{t}}{B_{t}+b_{t}}\right)$
9. ​    for $n=0$ to $N_t-1$ do
10. ​         Sample $b_t$ trajectories $\left\{\tau_{n, j}^{t}\right\}_{j=1}^{b_{t}}$ from $p\left(\cdot \mid \boldsymbol{\theta}_{n}^{t}\right)$
11. ​         $v_{n}^{t} \triangleq \frac{1}{b_{t}} \sum_{j=1}^{b_{t}}\left[g\left(\tau_{n, j}^{t} \mid \boldsymbol{\theta}_{n}^{t}\right)-\omega\left(\tau_{n, j}^{t} \mid \boldsymbol{\theta}_{n}^{t}, \boldsymbol{\theta}_{0}^{t}\right) g\left(\tau_{n, j}^{t} \mid \boldsymbol{\theta}_{0}^{t}\right)\right]+\mu_{t}$
12. ​     $\tilde{\boldsymbol{\theta}}_{t} \leftarrow \boldsymbol{\theta}_{N_{t}}^{t}$
13. **Output:** $\tilde{\boldsymbol{\theta}}_{a}$ uniformly randomly picked from $\left\{\tilde{\boldsymbol{\theta}}_{t}\right\}_{t=1}^{T}$



**Algorithm 2 FedPG-Dec**

1. **Input:** $z_{0}^{(k)} \in \mathbb{R}^{d}$, batch size $B_t$, mini batch size $b_t$, step size $\eta_t$
2. Initialize: $w_1^{(k)}=1$ for all nodes $k\in\{1,2,\dots,n\}$, values of $P(t)=[p_{k,i}(t)]_{n\times n}$
3. for $t=1$ to $T$, at node $k$, do
4. ​    $\boldsymbol{\theta}_{t,0}^{(k)}=z_{t-1}^{(k)}$
5. ​    Sample $B_t$ trajectories $\left\{\tau_{t, i}^{(k)}\right\}_{i=1}^{B_{t}}$ from $p\left(\cdot \mid \boldsymbol{\theta}_{t,0}^{(k)}\right)$
6. ​    $\mu_{t}^{(k)} \triangleq\frac{1}{B_{t}} \sum_{i=1}^{B_{t}} g\left(\tau_{t, i}^{(k)} \mid \boldsymbol{\theta}_{t,0}^{(k)}\right)$
7. ​    Sample $N_{t} \sim \operatorname{Geom}\left(\frac{B_{t}}{B_{t}+b_{t}}\right)$
8. ​    for $n=0$ to $N_t-1$ do
9. ​         Sample $b_t$ trajectories $\left\{\tau_{t, j}^{n,(k)}\right\}_{j=1}^{b_{t}}$ from $p\left(\cdot \mid \boldsymbol{\theta}_{t,n}^{(k)}\right)$
10. ​         $v_{t,n}^{(k)} \triangleq \frac{1}{b_{t}} \sum_{j=1}^{b_{t}}\left[g\left(\tau_{t, j}^{n,(k)} \mid \boldsymbol{\theta}_{t,n}^{(k)}\right)-\omega\left(\tau_{t, j}^{n,(k)} \mid \boldsymbol{\theta}_{t,n}^{(k)}, \boldsymbol{\theta}_{t,0}^{(k)}\right) g\left(\tau_{t, j}^{n,(k)} \mid \boldsymbol{\theta}_{t,0}^{(k)}\right)\right]+\mu_t^{(k)}$
11. ​         $\boldsymbol{\theta}_{t,n+1}^{(k)}=\boldsymbol{\theta}_{t,n}^{(k)}+\eta_tv_{t,n}^{(k)}$
12. ​    Send ($p_{i,k}(t)\boldsymbol{\theta}_{t,N_t}^{(k)}$,$p_{i,k}(t)w_t^{(k)}$) to out-neighbors, receive ($p_{k,i}(t)\boldsymbol{\theta}_{t,N_t}^{(i)}$,$p_{k,i}(t)w_t^{(i)}$) from in-neighbors
13. ​    $x_t^{(k)}=\sum_{i}p_{k,i}(t)\boldsymbol{\theta}_{t,N_t}^{(i)}$, $y_t^{(k)}=\sum_{i}p_{k,i}(t)w_t^{(i)}$, $z_t^{(k)}=x_t^{(k)}/y_t^{(k)}$
14. **Output:** $\theta_a^{(k)}$ uniformly randomly picked from $\{z_t^{(k)}\}^T_{t=1}$



## 4 Experiments

### 4.1 Centralized setting

我们对Centralized federated policy gradient with byzantine resilience (CenFPG-BR)在有拜占庭攻击和无拜占庭攻击两种情况下进行性能测试，测试的任务是CartPole balancing，其中每个智能体在采样步中会采集10个轨迹，结果取10次独立运行的平均值。W表示智能体的总数，B表示拜占庭智能体的数量。

我们首先评估CenFPG-BR在W=1、W=10和B=0、W=10和B=3上使用random-noise攻击的表现。random-noise攻击指的是拜占庭智能体发送一个随机向量给服务器。【image1】显示了多个智能体也就是联邦学习的使用，能帮助模型更快收敛。值得注意的是，即使引入了B=3的拜占庭智能体，CenFPG-BR的性能依然没有受到太大的影响，说明拜占庭过滤器起到了作用。

然后，我们评估在CenFPG-BR中使用random-action攻击的表现。random-action攻击指的是拜占庭智能体忽视服务器发来的策略而随机采取动作。【image2】显示的结论与前面类似，多智能体的训练效果好于单智能体，而拜占庭攻击不会对CenFPG-BR的性能造成严重影响，拜占庭攻击的出现轻微破坏了收敛后的稳定性。

【image3】和【image4】分别显示了W=10和B=3在random-noise和random-action攻击下使用CenFPG-BR独立运行10次的总体表现，可以看出CenFPG-BR方法在有中心场景下的表现较好。



## 5 Conclusion and future work



## References





