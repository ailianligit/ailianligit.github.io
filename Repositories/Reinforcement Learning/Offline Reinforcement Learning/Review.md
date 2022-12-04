# 离线强化学习综述

## 前言

​        强化学习是机器学习的一个子领域。在强化学习的过程中，智能体（agent）与环境进行交互，学习出一套能够最大化累积奖励（reward）的策略（policy）。通过对当前获取的知识进行开发，智能体能够学习得到相对较好的策略，但这个策略并不一定是全局最优的，智能体需要在未知的领域上探索更好的策略。因此，强化学习的研究重点是在探索（exploration）和开发（exploitation）之间做出取舍和平衡。

​        On-policy和Off-policy是强化学习领域两个非常重要的方法。其中，On-policy方法试图评估并直接改进用于做出决策的策略，智能体采样所用的策略和学习的目标策略一致，在同步时需要重新采样；而Off-policy方法评估或改进与用于生成数据的策略不同的策略，其收敛速度慢，但是采样效率相对较高，学习得到的策略通常具有更好的表现。

​        传统的强化学习本质上是一种启发式的算法，智能体需要不断探索未知的领域来强化已知的知识，然而在探索过程中，大部分的知识是无用的，因此强化学习是一类采样效率低下的方法。此外，在医疗保健、自动驾驶等场景中，我们不能允许智能体进行随意的探索，因为采样的代价十分昂贵，智能体采取探索性的策略可能会危及自身的安全。

​        离线强化学习（Offline RL）舍弃了与环境的交互，让智能体在一个固定的数据集（buffer）上进行训练，由此避免了传统强化学习需要与环境进行交互的限制，从而增大了强化学习算法在现实场景中应用的可能。离线强化学习方法建立在Off-policy方法的基础上，不同之处在于Off-policy方法仍然保留了与环境的交互，这些额外的数据能帮助智能体探索未知的领域和更好的策略。在线强化学习、Off-policy强化学习和离线强化学习的联系和对比如图1所示。

![image-20220416115604459](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220416115604459.png)



## 离线强化学习介绍

​        离线强化学习需要学习算法从固定的数据集中获得对马尔可夫决策过程背后的动态系统（状态转移、奖励）的充分理解，并构建一个在与实际的环境交互时，能够获得尽可能大的累积回报的策略。因此，离线强化学习的目标函数与传统强化学习的目标函数一致，都是方程（1）所表示的形式。其中，$\pi$表示训练得到的策略，$\mathbf{s}$表示状态（state），$\mathbf{a}$表示动作（action），$r$表示状态$\mathbf{s_t}$下采取动作$\mathbf{a_t}$的奖励（reward），$\gamma$表示折扣系数（discount factor），轨迹（trajectory）$\tau=(s_0,a_0,\dots,s_H,a_H)$的长度为$H$，$p_\pi$表示轨迹的分布。
$$
J(\pi)=\mathbb{E}_{\tau \sim p_{\pi}(\tau)}\left[\sum_{t=0}^{H}\gamma^{t} r\left(\mathbf{s}_{t}, \mathbf{a}_{t}\right)\right]\tag{1}
$$
​        在离线强化学习中，除了学习的策略$\pi(\mathbf{a} \mid \mathbf{s})$，还有行为策略（behavior policy）$\pi_\beta(\mathbf{a} \mid \mathbf{s})$，行为策略的含义是数据集$\mathcal{D}=\left\{\left(\mathbf{s}_{t}^{i}, \mathbf{a}_{t}^{i}, \mathbf{s}_{t+1}^{i}, r_{t}^{i}\right)\right\}$中状态$\mathbf{s}$和动作$\mathbf{a}$的分布，即对于所有$(\mathbf{s}, \mathbf{a}) \in \mathcal{D}$，$\mathbf{s} \sim d^{\pi_{\beta}}(\mathbf{s})$且$\mathbf{a} \sim \pi_{\beta}(\mathbf{a} \mid \mathbf{s})$，其中$d^{\pi_{\beta}}(\mathbf{s})$表示行为策略下状态访问的频率分布。对于目标函数（1），若策略$\pi$使用参数$\theta$表示，则其梯度可以用方程（2）来表示。
$$
\nabla_{\theta} J(\theta)=E_{\tau \sim \pi_{\theta}(\tau)}\left[\sum_{t=0}^{T} \nabla_{\theta} \gamma^{t} \log \pi_{\theta}\left(\mathbf{a}_{t} \mid \mathbf{s}_{t}\right) r\left(\mathbf{s}_{t}, \mathbf{a}_{t}\right)\right]
\approx\frac{1}{N}\sum_{i=1}^{N}\sum_{t=0}^{T}\nabla_{\theta}\gamma^{t}\log\pi_{\theta}\left(\mathbf{a}_{t, i}\mid\mathbf{s}_{t, i}\right)r\left(\mathbf{s}_{t, i},\mathbf{a}_{t,i}\right)\tag{2}
$$
​       由于离线强化学习在离线数据集上训练，因此这种方法也被称为数据驱动的强化学习（data-driven RL）和批处理强化学习（batch RL）。此外，完全Off-policy强化学习（fully off-policy RL）也是离线强化学习的别称，原因是去掉Off-policy中的在线采样过程，即可将在线学习变为离线学习。然而，并不是所有的在线方法变为离线后仍能保持高效的性能，因此离线强化学习算法的设计需要面临许多困难和挑战。

​        离线强化学习面临的核心挑战是分布偏移（distributional shift），通常指训练策略的分布偏离于行为策略的分布。由于离线训练的模型只对那些训练数据相同分布的输入是可靠的，因此当模型接收到不熟悉的输入，可能会高估其预期回报。在实际交互中，当智能体选择最大化预期回报时，便可能选到实际回报非常差的动作，这些动作称为分布外（out-of-distribution）的动作。此外，当数据集中不包含现实场景中高回报的区域时，离线强化学习方法无法学习到最好的策略。



## 经典的离线强化学习算法

### 基于重要性采样的策略梯度方法

​        对于方程（1）所表示的目标函数和方程（2）所表示的梯度，计算时需要对训练策略$\pi_\theta$进行采样，然而，固定的数据集只提供了对行为策略$\pi_\beta$的采样。因此，我们可以利用如方程（3）所示的重要性采样（importance sampling）的技术以及从行为策略$\pi_\beta$中采样的数据对梯度进行估计。其中$\hat{Q}\left(\mathbf{s}_{t}, \mathbf{a}_{t}\right)$表示对状态-动作对的$Q$值的估计。
$$
\nabla_{\theta} J(\theta) \approx \frac{1}{N} \sum_{i=1}^{N} \frac{\pi_{\theta}\left(\tau_{i}\right)}{\pi_{\beta}\left(\tau_{i}\right)} \sum_{t=0}^{T} \nabla_{\theta} \gamma^{t} \log \pi_{\theta}\left(\mathbf{a}_{t, i} \mid \mathbf{s}_{t, i}\right) \hat{Q}\left(\mathbf{s}_{t, i}, \mathbf{a}_{t, i}\right)
\tag{3}
$$
​      由于重要性权重需要在轨迹中的每个时间步进行乘积，因此由方程（3）得到的策略梯度可能会有较大的方差。为缓解这一问题，我们可以采用边缘化重要性采样的技术，即使用状态-边缘重要性比（state-marginal importance ratio）$w(\mathbf{s}, \mathbf{a})=\frac{d^{\pi}_\theta(\mathbf{s}, \mathbf{a})}{d^{\pi_\beta}(\mathbf{s}, \mathbf{a})}$来代替策略梯度中的重要性权重。状态-边缘重要性比的估计需要一致性条件来保证改变之后的策略梯度仍然有效，这其中涉及不动点的问题。



### 价值函数的估计和最小二乘策略迭代算法（LSPI）

​        为解决基于重要性采样方法中重要性权重指数级膨胀的问题，我们可以使用价值函数（value function）估计的方法将在线学习中的动态规划（dynamic programming）调整为离线的方法。动态规划方法的目标是学习一个状态价值函数$V$或状态-动作价值函数$Q$，然后使用这个价值函数来学习最优的策略（基于价值的方法）或者估计策略的期望回报的梯度（Actor-Critic算法）。在没有考虑引入神经网络对价值函数进行估计之前，基于线性函数的价值函数估计是一种久远但有效的方法。

​        使用线性函数对Q函数进行估计可以表示为$Q_\phi\approx\mathbf{f}(s,a)^T\phi$，其中$\mathbf{f}(s,a)$是与状态-动作对相关的特征向量。这个等式意味着我们可以使用行为策略$\pi_\beta(a\mid s)$和状态访问频率$d^{\pi_\beta}(s)$中采样的数据为训练策略$\pi(a\mid s)$估计参数化的Q函数$Q_\phi$。除此之外，我们还需要为价值函数的估计确定$\phi$。贝尔曼残差最小化方法使线性Q函数与贝尔曼方程表达的Q函数的二次误差尽可能小来获取精确的$\phi$。最小二次不动点近似方法则利用贝尔曼方程的迭代形式和投影贝尔曼算子获得方程（4）所示的迭代形式的$\phi$，其中$\mathbf{F}$是$\mathbf{f}(s,a)$的矩阵形式。
$$
\phi_{k+1}=\left(\mathbf{F}^{T} \mathbf{F}\right)^{-1} \mathbf{F}^{T}\left(\vec{R}+\gamma P^{\pi} \mathbf{F} \phi_{k}\right)\tag{4}
$$
​        最小二乘时间差分Q学习（Least squares temporal difference Q-learning, LSTD-Q）利用方程（4）来估计$\phi$，进而利用估计的Q函数进行训练。最小二乘策略迭代（Least squares policy iteration, LSPI）使用LSTD-Q作为近似策略评估的中间过程，然后利用估计的$Q\phi$和贪心策略得到下一次的策略迭代：$\pi_{k+1}(\mathbf{a} \mid \mathbf{s})=\delta\left(\mathbf{a}=\arg \max _{\mathbf{a}} Q_{\phi}(\mathbf{s}, \mathbf{a})\right)$。然而，由于行为策略$\pi_\beta$和训练策略$\pi$在训练过程中会产生偏差，因此几乎所有使用价值函数估计的动态规划算法都会受到第二节中提到的分布偏移的影响。



## 基于策略约束的方法

​       当只通过离线数据进行学习的时候，由于外推误差（extrapolation error）的存在，大多数Off-policy算法都将失败。这是由于离线数据之外的状态-动作对可能具有不准确的Q值，这对于依赖于Q值的算法会产生不利的影响。在标准的在线学习中，采用探索可以纠正这些误差，因为智能体可以得到真实的奖励进行修正，但在离线设定下的Off-policy方法缺乏这种纠错能力。 

​       基于约束策略的方法可以限制训练策略与行为策略之间的偏差，降低计算目标Q函数值时出现分布外的动作的可能性，从而减小分布的偏移。除此之外，为了在训练中改进策略，我们需要保持较小的偏差，控制一定数量的分布外动作，我们可以将具有策略约束的策略迭代方法表示为以下形式的不动点迭代方法。
$$
\begin{aligned}
\hat{Q}_{k+1}^{\pi} & \leftarrow \arg \min _{Q} \mathbb{E}_{\left(\mathbf{s}, \mathbf{a}, \mathbf{s}^{\prime}\right) \sim \mathcal{D}}\left[\left(Q(\mathbf{s}, \mathbf{a})-\left(r(\mathbf{s}, \mathbf{a})+\gamma \mathbb{E}_{\mathbf{a}^{\prime} \sim \pi_{k}\left(\mathbf{a}^{\prime} \mid \mathbf{s}^{\prime}\right)}\left[\hat{Q}_{k}^{\pi}\left(\mathbf{s}^{\prime}, \mathbf{a}^{\prime}\right)\right]\right)\right)^{2}\right] \\
\pi_{k+1} & \leftarrow \arg \max _{\pi} \mathbb{E}_{\mathbf{s} \sim \mathcal{D}}\left[\mathbb{E}_{\mathbf{a} \sim \pi(\mathbf{a} \mid \mathbf{s})}\left[\hat{Q}_{k+1}^{\pi}(\mathbf{s}, \mathbf{a})\right]\right] \text { s.t. } D\left(\pi, \pi_{\beta}\right) \leq \epsilon
\end{aligned}
$$

​       

### 批处理约束深度Q学习算法（BCQ）

​        批处理约束深度Q学习（Batch constrained deep Q-learning, BCQ）算法基于在线的Q学习算法，限制策略选择的状态-动作对符合离线数据集中数据的分布。BCQ算法在选取最大化Q值对应的动作时，只考虑行为策略$\pi_\beta$可能产生的动作。具体来说，BCQ算法训练一个生成模型（VAE）$G_\omega(s)$生成可能来自于离线数据的动作$a_i$，此外，还需要训练一个扰动模型（perturbation network）$\xi_{\phi}\left(s, a_{i}, \Phi\right)$对生成的动作产生干扰，其扰动的范围是$[-\Phi, \Phi]$，然后选择Q值最高的策略。这一过程可用方程（5）来表示。
$$
\pi(s)= \underset{a_{i}+\xi_{\phi}\left(s, a_{i}, \Phi\right)}{\operatorname{argmax}} Q_{\theta}\left(s, a_{i}+\xi_{\phi}\left(s, a_{i}, \Phi\right)\right),\left\{a_{i} \sim G_{\omega}(s)\right\}_{i=1}^{n} \tag{5}
$$

$$
\phi \leftarrow \underset{\phi}{\operatorname{argmax}} \sum_{(s, a) \in \mathcal{B}} Q_{\theta}\left(s, a+\xi_{\phi}(s, a, \Phi)\right)\tag{6}
$$

$$
y=r+\gamma \max _{a_{i}}\left[\lambda \min _{j=1,2} Q_{\theta_{j}^{\prime}}\left(s^{\prime}, a_{i}\right)+(1-\lambda) \max _{j=1,2} Q_{\theta_{j}^{\prime}}\left(s^{\prime}, a_{i}\right)\right]\tag{7}
$$

​        方程（6）表示的是扰动模型$\xi_{\phi}\left(s, a_{i}, \Phi\right)$的训练过程。为了对动作的Q值进行估计，BCQ算法训练了两个Q值网络，分别表示为$Q_{\theta_{1}^{\prime}}$和$Q_{\theta_{2}^{\prime}}$，取两个Q值网络的最小值作为价值目标$y$，表示为方程（7）的形式。具体的算法如图2所示。

![image-20220416214246567](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220416214246567.png)

​        对传统的Off-policy算法、模仿学习算法和BCQ算法的性能进行测试，并将实验的结果与数据集中的行为策略进行比较，结果如图3所示。BCQ的平均回报值高并且很快稳定，其表现优于传统的Off-policy算法和模仿学习算法，这说明本节开头提到的外推误差得到了很好的解决。此外，相较于一般的深度强化学习算法，BCQ算法需要的迭代次数明显减少。

![image-20220416221259437](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220416221259437.png)



### 自举误差累积缩减算法（BEAR）

​        在离线强化学习中，BCQ算法使用的VAE和扰动模型虽然解决了外推误差的问题，但对于一些处于行为策略分布外的状态-动作无法很好的拟合，自举误差累积缩减算法（Bootstrapping error accumulation reduction, BEAR）通过支持集匹配（supporting-set matching）的技术缩减了训练策略和行为策略之间的自举误差（bootstrapping error），实现了比BCQ算法更好的效果。

​        自举误差是通过自举训练数据分布外动作产生的，并通过贝尔曼回溯算子（Bellman backup operator）累积。在图4的左图中，红色的虚线表示由行为策略$\pi_\beta$得到的动作分布，在测试中我们可能得到如蓝线表示的分布外动作，而这些动作的Q值很高，在Q学习的自举过程中就会称为更新的目标，从而导致自举误差的累积。图4中间的图显示了BCQ算法中使用的分布匹配约束，紫色的线表示BCQ算法学到的接近行为策略动作分布（红色虚线）的动作分布。由于BCQ算法的策略约束方式类似于行为克隆（behavior cloning），因此BCQ算法在行为策略水平较高的数据集上表现较好，而在其他数据集上可能学到比较随机的策略。

![s_03D8A88577B961181603AE5EDBD4A511CD8E828E7651B8AA640A61950DAB9783_1575540392160_all_three_cases](D:\OneDrive - 中山大学\s_03D8A88577B961181603AE5EDBD4A511CD8E828E7651B8AA640A61950DAB9783_1575540392160_all_three_cases.png)

​        BEAR算法使用支持集匹配约束实现对策略的约束，本质上支持约束是通过训练策略的上界集中性来控制自举误差的累积，同时减少训练策略与最优策略之间的散度。图4的右图显示了BEAR算法使用的支持约束有潜力学习到一些接近最优的策略（黄线）。在具体是线上，BEAR算法使首先用方程（8）来最大化$\Pi_{\epsilon}$中策略$\pi$的Q值的保守估计，从而更新训练策略$\pi_\phi$。
$$
\pi_{\phi}(s):=\max _{\pi \in \Pi_{\epsilon}} \mathbb{E}_{a \sim \pi(\cdot \mid s)}\left[\min _{j=1, . ., K} \hat{Q}_{j}(s, a)\right]\tag{8}
$$

$$
\operatorname{MMD}^{2}\left(\left\{x_{1}, \cdots, x_{n}\right\},\left\{y_{1}, \cdots, y_{m}\right\}\right)=\frac{1}{n^{2}} \sum_{i, i^{\prime}} k\left(x_{i}, x_{i^{\prime}}\right)-\frac{2}{n m} \sum_{i, j} k\left(x_{i}, y_{j}\right)+\frac{1}{m^{2}} \sum_{j, j^{\prime}} k\left(y_{j}, y_{j^{\prime}}\right)\tag{9}
$$

$$
\pi_{\phi}:=\max _{\pi \in \Delta|S|} \mathbb{E}_{s \sim \mathcal{D}} \mathbb{E}_{a \sim \pi(\cdot \mid s)}\left[\min _{j=1, . ., K} \hat{Q}_{j}(s, a)\right] \text { s.t. } \mathbb{E}_{s \sim \mathcal{D}}[\operatorname{MMD}(\mathcal{D}(s), \pi(\cdot \mid s))] \leq \varepsilon\tag{10}
$$

​        BEAR算法使用最大平均差异距离（MMD）来近似衡量支持散度（support divergence），也就是训练策略$\pi$和行为策略$\pi_\beta$之间的距离。对于向量$X$、$Y$和任意的核函数$k$，MMD可以用方程（9）来表示。对于Actor-Critic算法，其中的策略改进步骤可以用方程（10）来表示。BEAR和Q学习结合的算法（BEAR-QL）如图5所示。

![image-20220418174439192](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220418174439192.png)

​        图6显示了BEAR-QL算法、BCQ算法、普通RL算法和BC算法在随机策略收集的数据（random data）和专家策略收集的数据（optimal data）上的表现，可以看出BCQ算法通常在专家策略收集的数据上表现更好，而BEAR算法在较弱甚至是随机策略收集的数据上有更好的表现。这一结果验证了前文对两种匹配约束方式和两种算法的理论分析。

![image-20220418201310150](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220418201310150.png)



## 讨论与观点

​       离线强化学习提供了将环境交互式强化学习转变为数据驱动式强化学习的可能性，这种转变有助于利用大规模的数据和目前应用广泛的监督学习思想来提升强化学习训练的性能。然而，这样的转变需要将统计方法上的创新与传统的强化学习顺序决策方法相结合，例如文中提到的重要性采样方法、自举误差等等。

​        离线强化学习的核心是一个反事实推理问题：给定一组行为决策产生的数据，使用训练的模型去推断出另外一组不同决策的结果。这类问题在机器学习方法中极具挑战性，因为我们不能再要求训练和测试的数据之间具有相同且独立的分布。除了文中提到的基于策略约束的方法，基于正则化、基于不确定性估计或基于模型等方法都是为了解决分布偏移的问题。

​        在强化学习领域，除了Off-policy和On-policy这两大类方法，还有基于模型和无模型这两类强化学习方法。本文提到的所有算法都是基于无模型的方法，因为在通常的认知中，真实环境的模型是难以学习的。然而，基于模型的离线强化学习具有便于评估、探索性强、能做全局约束等无模型方法不具备的优势，因此基于模型的方法将是离线强化学习未来发展的重要部分。



## 结论

​        离线强化学习由标准的Off-policy方法发展而来。重要性采样方法是基于策略的强化学习在离线的设置上对于未知的行为策略的估计，由于重要性权重的累乘，这种方法通常具有较大的误差，可通过边缘化重要性采样或价值函数估计的方法缓解。价值函数的估计是基于价值的强化学习在离线问题上的自然延伸，由此得到LSTD-Q和LSPI这两种线性价值函数估计的动态规划算法。然而，几乎所有基于价值函数估计的动态规划算法都会受到分布偏移的影响，而这也是现代离线强化学习的核心挑战。

​        随着神经网络等非线性估计器的广泛应用，现代的离线强化学习方法已不再局限于用线性函数去估计价值函数，而是专注于用各种统计和优化的思想去解决分布偏移的问题。BCQ算法是一种基于策略约束的方法，该方法通过分布匹配约束来限制外推误差，而由于该思想与行为克隆的相似性，其在行为策略水平较高的数据集上才能有较好的表现。在这之后提出的BEAR算法通过支持集匹配约束来限制自举累积误差，因此在各种水平的数据集上都能有效训练。

​        除了经典的离线强化学习算法和基于策略约束的方法，现代的离线强化学习还包括基于正则化、基于不确定性估计和基于模型的方法，由于本人的能力和篇幅所限，暂时无法将所有的离线强化学习算法类型归纳详尽。虽然受到分布偏移等挑战，离线强化学习因其数据驱动的特点，有效解决了传统强化学习在实际场景中采样效率低、采样成本高昂的问题，促进了强化学习方法在生产生活各领域的落地和应用。



## 参考文献

