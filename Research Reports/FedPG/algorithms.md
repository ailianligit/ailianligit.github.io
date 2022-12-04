**Algorithm 2 FedPG-Dec**

1. **Input:** $\boldsymbol{\phi}_{1,0}^{(k)}=\boldsymbol{\theta}_{1,0}^{(k)} \in \mathbb{R}^{d}$, batch size $B_t$, mini batch size $b_t$, step size $\eta_t$
2. Initialize: $w_1^{(k)}=1$ for all nodes $k\in\{1,2,\dots,K\}$, values of $P=[p_{k,i}]_{K\times K}$ (column stochastic)
3. for $t=1$ to $T$ do
4. ​    Sample $N_{t} \sim \operatorname{Geom}\left(\frac{B_{t}}{B_{t}+b_{t}}\right)$
5. ​    for each node $k$ parallelly do
6. ​        Sample $B_t$ trajectories $\left\{\tau_{t, i}^{(k)}\right\}_{i=1}^{B_{t}}$ from $p\left(\cdot \mid \boldsymbol{\theta}_{t,0}^{(k)}\right)$
7. ​        $\mu_{t}^{(k)} \triangleq\frac{1}{B_{t}} \sum_{i=1}^{B_{t}} g\left(\tau_{t, i}^{(k)} \mid \boldsymbol{\theta}_{t,0}^{(k)}\right)$
8. ​        for $n=0$ to $N_t-1$ do
9. ​            Sample $b_t$ trajectories $\left\{\tau_{t, j}^{n,(k)}\right\}_{j=1}^{b_{t}}$ from $p\left(\cdot \mid \boldsymbol{\theta}_{t,n}^{(k)}\right)$
10. ​            $v_{t,n}^{(k)} \triangleq \frac{1}{b_{t}} \sum_{j=1}^{b_{t}}\left[g\left(\tau_{t, j}^{n,(k)} \mid \boldsymbol{\theta}_{t,n}^{(k)}\right)-\omega\left(\tau_{t, j}^{n,(k)} \mid \boldsymbol{\theta}_{t,n}^{(k)}, \boldsymbol{\theta}_{t,0}^{(k)}\right) g\left(\tau_{t, j}^{n,(k)} \mid \boldsymbol{\theta}_{t,0}^{(k)}\right)\right]+\mu_t^{(k)}$
11. ​            $\boldsymbol{\phi}_{t,n+1}^{(k)}=\boldsymbol{\phi}_{t,n}^{(k)}+\eta_tv_{t,n}^{(k)}$, $\boldsymbol{\theta}_{t,n+1}^{(k)}=\boldsymbol{\phi}_{t,n+1}^{(k)}/w_{t}^{(k)}$
12. ​    for each node $k$ parallelly do
13. ​        Send ($p_{i,k}\boldsymbol{\phi}_{t,N_t}^{(k)}$, $p_{i,k}w_t^{(k)}$) to out-neighbors $i$, receive ($p_{k,i}\boldsymbol{\phi}_{t,N_t}^{(i)}$, $p_{k,i}w_t^{(i)}$) from in-neighbors $i$
14. ​        $\boldsymbol{\phi}_{t+1,0}^{(k)}=\sum_{i}p_{k,i}\boldsymbol{\phi}_{t,N_t}^{(i)}$, $w_{t+1}^{(k)}=\sum_{i}p_{k,i}w_t^{(i)}$, $\boldsymbol{\theta}_{t+1,0}^{(k)}=\boldsymbol{\phi}_{t+1,0}^{(k)}/w_{t+1}^{(k)}$
15. **Output:** $\boldsymbol{\theta}_{T+1,0}^{(k)}$