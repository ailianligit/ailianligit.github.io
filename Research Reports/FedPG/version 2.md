# Distributed Policy Gradient for Federated Reinforcement Learning

## Abstract

联邦强化学习让多个智能体在不共享raw trajectories的情况下，利用更多的数据build a better decision-making policy。尽管中心化的联邦强化学习更便于收集数据和构建策略，但是由于通信效率的限制，非中心的联邦强化学习有更广泛的应用场景。在中心化的联邦策略梯度的基础上，我们提出了基于有向图的联邦策略梯度的非中心版本。我们比较了两种算法在多个智能体同时进行CartPole任务时的性能。



## 1 Introduction

【Introduction补充这两段】

**Application.** 当强化学习应用于临床决策时，由于每家医院拥有的病例数有限，因此难以训练出有效的治疗方案。一种自然的解决方案是让多个RL智能体（e.g., different hospitals）共享他们的trajectories，然而，raw RL trajectories包含许多敏感信息(e.g., the medical records contain sensitive information about patients) ，因此我们可以结合联邦学习对隐私的保护机制。

在电量资源分配的场景中，由于电量的运输需要高昂的成本，因此智能体（e.g., 发电站）之间的通信效率受到较大的限制。由于智能体不需要与中心服务器进行通信，并且智能体只与相邻的智能体交换信息，因此非中心化的联邦强化学习在通信效率方面有巨大的优势。



## 2 Related Works

【Related Work替换为下面这段，原来的内容删掉，我把那些转移到第3节了】

Flint Xiaofeng Fan et al. 提出了带有拜占庭弹性的中心化的联邦策略梯度算法FedPG-BR，基于FedPG-BR，我们不考虑agents的随机故障和对抗性攻击，提出了中心化的联邦策略梯度算法FedPG-Cen。在非中心的场景下，由于不同智能体之间的信息需要聚合以实现联合学习的效果，因此我们应用了Mahmoud Assran   et al. 为分布式深度学习提出的stochastic gradient update方法，其中的Stochastic Gradient Push算法来源于David Kempe等人提出的Push-Sum思想。Zhaoxian Wu et al.提出的decentralized TD learning with linear function approximation对非中心场景下另外一种RL算法TD learning进行了分析。Pengcheng Dai et al.提出的distributed actor–critic algorithms over directed graphs对有向图场景下的AC算法进行了设计与分析，我们的非中心化联邦策略梯度算法FedPG-Dec也将应用在有向图的场景下。



## 3 Federated Policy Gradient

### 3.1 Centralized Version

#### 3.1.1 Federated learning setting

在中心化的联邦学习设置中，$K$个分布式智能体表示为$k\in\{1,\dots,K\}$。在每一轮$t\in\{1,\dots,T\}$，中心服务器将策略网络的参数$\boldsymbol{\theta}_0^t$广播给所有的智能体。每个智能体使用获得的策略独立地与环境交互，采集一批（batch）轨迹（trajectories）$\{\tau_{t,i}^{(k)}\}_{i=1}^{B_t}\sim p(\cdot\mid\boldsymbol{\theta}_0^t)$。智能体根据本地轨迹计算的估计梯度$\mu_{t}^{(k)}$会直接发送给服务器。

服务器对所有智能体的梯度进行平均聚合。为了trade-off between convergence rate and per-iteration computational cost，我们在Stochastic Variance-Reduced的框架下进行多轮迭代，实现方差缩减的策略梯度上升。由于强化学习存在Non-stationarity的问题，我们利用重要性采样进行参数更新。



#### 3.1.2 Policy gradient estimator

PG methods are generally more effective in high-dimensional problems and enjoy the flexibility of stochasticity. 策略梯度的目标是最大化期望回报$\mathbb{E}_{\tau \sim p(\cdot \mid \boldsymbol{\theta})}[\mathcal{R}(\tau)]$，其中$\theta$表示策略网络的参数，通过梯度上升的方法更新。策略梯度可以表示为：
$$
\nabla_{\boldsymbol{\theta}} J(\boldsymbol{\theta})=\int_{\tau} \mathcal{R}(\tau) \nabla_{\boldsymbol{\theta}} p(\tau \mid \boldsymbol{\theta}) \mathrm{d} \tau=\mathbb{E}_{\tau \sim p(\cdot \mid \boldsymbol{\theta})}\left[\nabla_{\boldsymbol{\theta}} \log p(\tau \mid \boldsymbol{\theta}) \mathcal{R}(\tau)\right]
$$
因为策略梯度难以直接计算，因此随机梯度上升是更加实际的选择，随机策略梯度的估计表示为：
$$
\widehat{\nabla}_{B}^{} J(\boldsymbol{\boldsymbol{\theta}})=\frac{1}{B} \sum_{i=1}^{B} g\left(\tau_{i} \mid \boldsymbol{{\theta}}\right)
$$
其中$g\left(\tau \mid \boldsymbol{{\theta}}\right)$是$\nabla_{\boldsymbol{{\theta}}} \log p(\tau \mid \boldsymbol{{\theta}}) \mathcal{R}(\tau)$的无偏估计。策略梯度的REINFORCE estimator表示为：
$$
g(\tau \mid \boldsymbol{\theta})=\left[\sum_{h=0}^{H-1} \nabla_{\boldsymbol{\theta}} \log \pi_{\boldsymbol{\theta}}\left(a_{h} \mid s_{h}\right)\right]\left[\sum_{h=0}^{H-1} \gamma^{h} \mathcal{R}\left(s_{h}, a_{h}\right)-C_{b}\right]
$$
其中$C_b$是corresponding baseline。最终，智能体发送给服务器的梯度表示为$\mu_{t}^{(k)} \triangleq \widehat{\nabla}_{B_t}^{(k)} J(\boldsymbol{\boldsymbol{\theta}_{0}^{t}})$。



#### 3.1.3 Stochastic variance-reduced gradient

在使得目标函数最小的优化问题中，常用的优化算法有梯度下降（GD）和随机梯度下降（SGD）。GD能够实现linear convergence，然而，当数据集很大时，GD每次迭代需要巨大的计算成本。SGD降低了每次迭代的计算成本，但只有sub-linear convergence rate。stochastic variancereduced gradient (SVRG) 通过重用过去的梯度减少了当前梯度估计的方差，从而实现了trade-off between convergence rate and per-iteration computational cost。stochastically controlled stochastic gradient (SCSG)进一步降低了SVRG的计算成本。



#### 3.1.4 Non-stationarity and Importance Sampling

与监督学习不同，RL trajectories的分布受到策略参数的影响，而策略的参数随时间变化(e.g., $\tau \sim p(\cdot \mid \boldsymbol{\theta})$)，因此我们需要利用重要性采样技术解决Non-stationarity的问题。



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



### 3.2 Decentralized Version Over Directed Graphs

#### 3.2.1 Federated learning setting

在无中心的联邦学习中，我们采用了有向图的设置。$K$个分布式智能体表示为$k\in\{1,\dots,K\}$，智能体之间的weight matrix表示为$P(t)=[p_{i,j}(t)]_{K\times K}$，在我们的设置中，$P(t)$是一个row stochastic矩阵。weight matrix带有参数$t$意味着有向图的网络拓扑可能会随着时间$t$发生变化。在每一轮$t\in\{1,\dots,T\}$，每个智能体使用本地的策略$\boldsymbol{\theta}_{t,0}^{(k)}$独立地与环境交互，采集一批（batch）轨迹（trajectories）$\{\tau_{t,i}^{(k)}\}_{i=1}^{B_t}\sim p\left(\cdot \mid \boldsymbol{\theta}_{t,0}^{(k)}\right)$。智能体根据本地轨迹计算得到估计的梯度$\mu_{t}^{(k)}$。

在Stochastic Variance-Reduced的框架下进行多轮迭代，实现方差缩减的策略梯度上升，得到更新的本地参数$\boldsymbol{\theta}_{t,n+1}^{(k)}$。在这之后，利用push-sum protocol对智能体及其邻近的智能体进行参数的聚合，最终得到最新的本地参数$z_t^{(k)}$。



#### 3.2.2 Push-sum protocol

在每一轮$t\in\{1,\dots,T\}$，智能体会将自己的参数和权重相乘得到$p_{i,k}(t)\boldsymbol{\theta}_{t,N_t}^{(k)}$，将另一个参数$w_t^{(k)}$和权重相乘得到$p_{i,k}(t)w_t^{(k)}$，并将这两个数据发送给自己的邻居，同时智能体也会收到来自相邻智能体的数据，分别累和得到$x_t^{(k)}$和$y_t^{(k)}$。我们将$z_t^{(k)}=x_t^{(k)}/y_t^{(k)}$作为更新的参数。在每一轮中，由于所有智能体的$x_t^{(k)}$之和始终为参数之和，所有智能体的$y_t^{(k)}$之和始终为智能体的数量$K$，因此在多轮迭代后，最终同时实现参数更新和聚合的效果。



**Algorithm 2 FedPG-Dec**

1. **Input:** $z_{0}^{(k)} \in \mathbb{R}^{d}$, batch size $B_t$, mini batch size $b_t$, step size $\eta_t$
2. Initialize: $w_1^{(k)}=1$ for all nodes $k\in\{1,2,\dots,K\}$, values of $P(t)=[p_{k,i}(t)]_{K\times K}$
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



## 【其他的部分不用动】