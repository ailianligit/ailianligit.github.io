# Reinforcement Learning with Value Function Approximation

## Value Function Approximation for Model-Free Prediction & Control

- MC: target is the actual return
  - Prediction: $\Delta \mathbf{w}=\alpha\left(G_{t}-\hat{v}\left(s_{t}, \mathbf{w}\right)\right) \nabla_{\mathbf{w}} \hat{V}\left(s_{t}, \mathbf{w}\right)$
  - Control: $\Delta \mathbf{w}=\alpha\left(G_{t}-\hat{q}\left(s_{t}, a_{t}, \mathbf{w}\right)\right) \nabla_{\mathbf{w}} \hat{q}\left(s_{t}, a_{t}, \mathbf{w}\right)$

- TD: target is the TD target
  - Prediction: $\Delta \mathbf{w}=\alpha\left(R_{t+1}+\gamma \hat{v}\left(s_{t+1}, \mathbf{w}\right)-\hat{v}\left(s_{t}, \mathbf{w}\right)\right) \nabla_{\mathbf{w}} \hat{v}\left(s_{t}, \mathbf{w}\right)$
  - Sarsa: $\Delta \mathbf{w}=\alpha\left(R_{t+1}+\gamma \hat{q}\left(s_{t+1}, a_{t+1}, \mathbf{w}\right)-\hat{q}\left(s_{t}, a_{t}, \mathbf{w}\right)\right) \nabla_{\mathbf{w}} \hat{q}\left(s_{t}, a_{t}, \mathbf{w}\right)$
  - Q-learning: $\Delta \mathbf{w}=\alpha\left(R_{t+1}+\gamma \max _{a} \hat{q}\left(s_{t+1}, a, \mathbf{w}\right)-\hat{q}\left(s_{t}, a_{t}, \mathbf{w}\right)\right) \nabla_{\mathbf{w}} \hat{q}\left(s_{t}, a_{t}, \mathbf{w}\right)$



## Linear Feature Representations

- Linear value function approximation:
  - value function: $\hat{v}(s, \mathbf{w})=\mathbf{x}(s)^{T} \mathbf{w}=\sum_{j=1}^{n} x_{j}(s) w_{j}$
  - objective function: $J(\mathbf{w})=\mathbb{E}_{\pi}\left[\left(v^{\pi}(s)-\mathbf{x}(s)^{T} \mathbf{w}\right)^{2}\right]$
  - update rule: $\Delta \mathbf{w}=\alpha\left(v^{\pi}(s)-\hat{v}(s, \mathbf{w})\right) \mathbf{x}(s)$



## Deep Q-Networks

### Experience Replay

- store transition $(s_t,a_t,r_t,s_{t+1})$ in replay memory $D$
- sample random mini-batch of experience tuple from the dataset: $(s,a,r,s')\sim D$
- to reduce the correlations among samples

### Fixed Q Targets

- $\Delta \mathbf{w}=\alpha\left(r+\gamma \max _{a^{\prime}} \hat{Q}\left(s^{\prime}, a^{\prime}, \mathbf{w}^{-}\right)-Q(s, a, \mathbf{w})\right) \nabla_{\mathbf{w}} Q(s, a, \mathbf{w})$

- to help improve stability

![image-20220302140010552](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220302140010552.png)

![image-20220302135923026](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220302135923026.png)