# Model-Free Reinforcement Learning

## Model-Free Prediction

### Monte Carlo Policy Evaluation

- $v^{\pi}(s)=\mathbb{E}_{\tau \sim \pi}\left[G_{t} \mid s_{t}=s\right]$

  - $\tau$ (trajectory/episode): $\left\{S_{1}, A_{1}, R_{1}, S_{2}, A_{2}, R_{2}, \ldots, S_{T}, A_{T}, R_{T}\right\}$

- MC updates the **empirical mean** return with **one** sampled episode

  - sample a lot of trajectories, compute the **actual** returns for all the trajectories, then average them

  - MC does not require MDP dynamics/rewards, no bootstrapping, and does not assume state is Markov
  - Only applied to **episodic** MDPs (each episode terminates)

- $v\left(S_{t}\right) \leftarrow v\left(S_{t}\right)+\alpha\left(G_{i, t}-v\left(S_{t}\right)\right)$

  - $G_{i,t}$: computed return of episode $i$

<img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079826.png" alt="image-20220302104602373" style="zoom:50%;" />



### Temporal Difference (TD) Learning

- TD learns **online** from incomplete episodes, by **bootstrapping**
  - TD works in **continuing** (non-terminating) environments
  - TD exploits **Markov** property, more efficient in Markov environments
- $v\left(S_{t}\right) \leftarrow v\left(S_{t}\right)+\alpha\left(R_{t+1}+\gamma v\left(S_{t+1}\right)-v\left(S_{t}\right)\right)$
  - TD target (**estimated** return): $R_{t+1}+\gamma v\left(S_{t+1}\right)$
  - TD error: $\delta_t=R_{t+1}+\gamma v\left(S_{t+1}\right)-v\left(S_{t}\right)$

<img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079824.png" alt="image-20220302105251831" style="zoom:50%;" />



## Model-Free Control

### MC Control

- $\epsilon$-Greedy Exploration: trade-off between exploration & exploitation
  - with probability 1-$\epsilon$ choose the greedy action
  - with probability $\epsilon$ choose an action at random

![image-20220302113811463](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079821.png)



### TD Control

#### Sarsa Algorithm: On-Policy

- learn about policy $\pi$ from the experience collected from $\pi$

![image-20220302114444617](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079819.png)

#### Q-Learning: Off-Policy

- learn about policy $\pi$ from the experience sampled from another policy $\mu$
  - $\pi$ (target policy): is being learned about & becomes the optimal policy, is greedy on $Q(s,a)$ ($A'$)
  - $\mu$: is more exploratory & is used to generate trajectories, is $\epsilon$-greedy on $Q(s,a)$ ($A$)

![image-20220302114753637](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079817.png)

<img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079816.png" alt="image-20220302120535684" style="zoom:50%;" />