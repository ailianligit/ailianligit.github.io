# Markov Decision Process

## Markov Processes

- Markov Process (Markov Chain): $\lang S,P\rang$
  - A state $S_t$ is Markov iff $\mathbb{P}\left[S_{t+1} \mid S_{t}\right]=\mathbb{P}\left[S_{t+1} \mid S_{1}, \ldots, S_{t}\right]$
    - episodes: $S_{1}, S_{2}, \ldots, S_{T}$
    - A set of states: $S$
  - State transition probability: $P_{ss'}=\mathbb{P}[S_{t+1}=s'|S_t=s]$
    - State transition matrix: $P$



## Markov Reward Processes

- Markov Reward Process: $\lang S,P,R,\gamma\rang$
  - reward function: $R_s=\mathbb{E}[R_{t+1}|S_t=s]$
  - discount factor: $\gamma \in[0,1]$

- Return: $G_t=\sum_{k=0}^{\infty}\gamma^kR_{t+k+1}$

- State-value function: $v(s)=\mathbb{E}[G_t|S_t=s]=R(s)+\gamma\sum_{s'\in S}P(s'|s)v(s')$


- Bellman Equation: $v(s)=R(s)+\gamma\sum_{s'\in S}P(s'|s)v(s')$



## Markov Decision Processes

- Markov Decision Process: $\lang S,A,P,R,\gamma \rang$
  - Action: $A_t$

- Policy: $\pi(a|s)=\mathbb{P}[A_t=a|S_t=s]$

  - policies are stationary: $A_{t} \sim \pi\left(\cdot \mid S_{t}\right), \forall t>0$

- State-value function: $v_{\pi}(s)=\mathbb{E}_{\pi}\left[G_{t} \mid S_{t}=s\right]$

  - Bellman Expectation Equation: $v_{\pi}(s)=\mathbb{E}_{\pi}\left[R_{t+1}+\gamma v_{\pi}\left(S_{t+1}\right) \mid S_{t}=s\right]=\sum_{a \in \mathcal{A}} \pi(a \mid s) q_{\pi}(s, a)$

    <img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079496.png" alt="image-20220301174018084" style="zoom: 50%;" />

  - Optimal state-value function: $v_{*}(s)=\max _{\pi} v_{\pi}(s)=\max _{a} q_{*}(s, a)$

- Action-value function: $q_{\pi}(s, a)=\mathbb{E}_{\pi}\left[G_{t} \mid S_{t}=s, A_{t}=a\right]$

  - Bellman Expectation Equation: $q_{\pi}(s, a)=\mathbb{E}_{\pi}\left[R_{t+1}+\gamma q_{\pi}\left(S_{t+1}, A_{t+1}\right) \mid S_{t}=s, A_{t}=a\right]=\mathcal{R}_{s}^{a}+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}}^{a} v_{\pi}\left(s^{\prime}\right)$

    <img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079507.png" alt="image-20220301174204954" style="zoom:50%;" />

  - Optimal action-value function: $q_{*}(s, a)=\max _{\pi} q_{\pi}(s, a)=\mathcal{R}_{s}^{a}+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}}^{a} v_{*}\left(s^{\prime}\right)$



## Policy Iteration

### Policy Evaluation

- **Iterative** state-value function: $v_{t+1}(s)=R^{\pi}(s)+\gamma P^{\pi}\left(s^{\prime} \mid s\right) v_{t}\left(s^{\prime}\right)=\sum_{a \in \mathcal{A}} \pi(a \mid s)\left(R(s, a)+\gamma \sum_{s^{\prime} \in \mathcal{S}} P\left(s^{\prime} \mid s, a\right) v_{t}\left(s^{\prime}\right)\right)$

  <img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079505.png" alt="image-20220301180346832" style="zoom:50%;" />



### Policy Improvement

- $q^{\pi_{i}}(s, a)=R(s, a)+\gamma \sum_{s^{\prime} \in S} P\left(s^{\prime} \mid s, a\right) v^{\pi_{i}}\left(s^{\prime}\right)$


- $\pi_{i+1}(s)=\underset{a}{\arg \max } q^{\pi_{i}}(s, a)$

  <img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079503.png" alt="image-20220301180829207" style="zoom:50%;" />



## Value Iteration

- **Iterative** action-value function: $q_{k+1}(s, a)=R(s, a)+\gamma \sum_{s^{\prime} \in S} P\left(s^{\prime} \mid s, a\right) v_{k}\left(s^{\prime}\right)$

- **Iterative** state-value function: $v_{k+1}(s)=\max _{a} q_{k+1}(s, a)$

- Optimal policy: $\pi(s)=\underset{a}{\arg \max } R(s, a)+\gamma \sum_{s^{\prime} \in S} P\left(s^{\prime} \mid s, a\right) v_{k+1}\left(s^{\prime}\right)$


![image-20220301235708648](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079501.png)
