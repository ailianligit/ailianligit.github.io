考核方式：写一份博士期间的Research proposal，方向就定在Visual model-based RL。
英文，长度3-4页，用overleaf的默认模板就可以。
需要包括相关前沿工作调研（最好能对主流方法做出归纳和对比）、热点开放问题汇总、你的博士生涯研究规划。

## Introduction

To improve the sampling efficiency and practical effect of traditional RL, model-based reinforcement learning (MBRL) builds environment models in which the trial-and-errors can take place without real costs. By acquiring predictive models that represent how the world works, agents can quickly derive effective policies by planning or by simulating synthetic experience under the model. In visual control tasks, agents must learn policies from high-dimensional observations, drawing out a promising direction called visual MBRL. The world model \cite{ha2018recurrent} provides an explicit way to represent the agent's knowledge of the world in a parametric model that can predict the future. However, challenges for visual MBRL still exist, such as modeling entire spaces with complex visual scenes, and learning dynamic models that are accurate enough for planning.

## World Models

**论文**

David Ha and Jürgen Schmidhuber. Recurrent world models facilitate policy evolution. In NeurIPS, 2018.

**摘要**

We explore building generative neural network models of popular reinforcement learning environments. Our *world model* can be trained quickly in an unsupervised manner to learn a compressed spatial and temporal representation of the environment. By using features extracted from the world model as inputs to an agent, we can train a very compact and simple policy that can solve the required task. We can even train our agent entirely inside of its own dream environment generated by its world model, and transfer this policy back into the actual environment.

我们探索建立流行的强化学习环境的生成神经网络模型。我们的世界模型可以以无监督的方式快速训练，以学习环境的压缩空间和时间表示。通过使用从世界模型中提取的特征作为代理的输入，我们可以训练一个非常紧凑和简单的策略来解决所需的任务。我们甚至可以完全在由其世界模型生成的自己的梦想环境中训练我们的代理，并将此策略转移回实际环境。

**引用**

Ha and Schmidhuber proposed the World Models [cite] that can be trained in an unsupervised manner to learn compressed latent states of the environment, and then train the agent entirely inside of its own dream environment generated by the world model.

**动机**

The inspiration of the World Models [cite] proposed by Ha and Schmidhuber is that, what we perceive at any given moment is governed by our brain's prediction of the future based on our internal model, which learns an abstract representation of both spatial and temporal aspects of the perceived information [cite].

**目的**

Large RNNs-based agents can learn rich spatial and temporal representations of data, however, millions of weights of a large model often cause credit assignment problem which limits the performance of the RL algorithms. The solution is to train a large world model in an unsupervised manner, and then train a small controller model to learn the policy using this world model.

**方法**

Specifically, the raw observation is first processed by the vision model ($V$), which encodes the high-dimensional observation into a low-dimensional latent vector. A small controller ($C$) uses the representations from both $V$ and the memory RNN ($M$) to select good actions. $M$ will then integrates the historical codes and actions to create a representation that can predict future states. Further, the World Models can train the agent entirely inside of dream environment generated by world model, and transfer the policy back into the actual environment.

**表现**

Experiments show that the World Models can be used to solve many challenging tasks that previously has not been solved using traditional RL methods, and the model that trained in the dream environment considers extra uncertainty and performs better than the normal setting.

**优劣**

The advantages and disadvantages of the World Models are obvious. Firstly, the combination of $V$ and $M$ enables the model to have predictive capabilities of both temporal and spatial aspects. After $V$ performs feature extraction, the effective information perceived by the agent increases, which accelerates $C$'s policy learning. Separating the small control model from the large world model helps increase the capacity and expressiveness of the model while avoiding unnecessary computation. In addition, training the agent in the dream environment can easily increase randomness and achieve better performance. The introduction of latent vectors and VAE decoder may improve the interpretability of the model. On the other hand, the limitation of the model is the unknown symbolism and the  insufficient readability of the extracted features, and the structure of the model is fixed and rigid, which cannot be updated with the latent vector.



## PlaNet

**论文**

Danijar Hafner, Timothy Lillicrap, Ian Fischer, Ruben Villegas, David Ha, Honglak Lee, and James Davidson. Learning latent dynamics for planning from pixels. In ICML, pages 2555–2565. PMLR, 2019.

**摘要**

Planning has been very successful for control tasks with known environment dynamics. To leverage planning in unknown environments, the agent needs to learn the dynamics from interactions with the world. However, learning dynamics models that are accurate enough for planning has been a long-standing challenge, especially in image-based domains. We propose the Deep Planning Network (PlaNet), a purely model-based agent that learns the environment dynamics from images and chooses actions through fast online planning in latent space. To achieve high performance, the dynamics model must accurately predict the rewards ahead for multiple time steps. We approach this using a latent dynamics model with both deterministic and stochastic transition components. Moreover, we propose a multi-step variational inference objective that we name latent overshooting. Using only pixel observations, our agent solves continuous control tasks with contact dynamics, partial observability, and sparse rewards, which exceed the difficulty of tasks that were previously solved by planning with learned models. PlaNet uses substantially fewer episodes and reaches final performance close to and sometimes higher than strong model-free algorithms.

对于具有已知环境动态的控制任务，规划非常成功。为了在未知环境中利用规划，智能体需要从与世界的交互中学习动态。然而，学习足够准确以进行规划的动态模型一直是一项长期挑战，尤其是在基于图像的领域。我们提出了深度规划网络（PlaNet），一个纯粹基于模型的智能体从图像中学习环境动态，并通过隐空间中的快速在线规划来选择动作。为了实现高性能，动态模型必须准确预测未来多个时间步的奖励。我们使用具有确定性和随机性转换分量的隐动态模型来解决这个问题。此外，我们提出了一个多步变分推理目标，我们将其命名为隐overshooting。仅使用像素观察，我们的代理解决了具有接触动态、部分可观察性和稀疏奖励的连续控制任务，这超过了以前通过已学习模型的规划解决的任务的难度。 PlaNet 使用更少的episodes，并达到接近甚至有时高于强大的无模型算法的最终性能。

**引用**

Danijar Hafner et al. proposed the Deep Planning Network (PlaNet) [cite], which learns the recurrent state-space model, optimizes the action policy on the recurrent states with the cross-entropy methods and trains multi-step predictions with a latent overshooting objective.

**动机**

Planning is a powerful approach when the rules of the environment are known, for example in AlphaGo. To plan in unknown environments, the agent needs to learn a world model over time from experience.

**方法**

The Deep Planning Network (PlaNet) [cite] proposed by Danijar Hafner et al. is a recipe for scalable model-based RL using latent planning. Specifically, a recurrent state space model predicts future images and rewards using a sequence of compact latent states, trained as a sequential VAE. PlaNet selects actions by simple population based search and replan at every step. To enable accurate long-term predictions, deterministic states pass information far into the future and stochastic states predict multiple futures, which guarantee partial observability and model uncertainty. Moreover, a latent overshooting objective has been proposed to generalize the standard variational bound to include multi-step predictions.

**与Model-free比较**

Compared to the model-free methods, PlaNet reaches the performance of top model-free agents in 200x fewer episodes and similar computation time on the tasks including partial observability, contact dynamics, sparse rewards, and many degrees of freedom. Moreover, planning carries the promise of increasing performance just by increasing the computational budget for searching for actions [silver2017mastering]. Experiments also show that PlaNet has the potential to transfer well to other tasks in the environment, because learned dynamics can be independent of any specific task.

**与World Models比较**

Compared to the World models, PlaNet also reconstruct images to provide a rich training signal and only need compact codes to accelerate the computation. However, PlaNet works without a policy network, which selects actions purely by planning and benefits from model improvements on the spot.

**劣势**

The limit of PlaNet is the fixed action repeat, which may be improved by hierarchical models.



## Dreamer

**论文**

Danijar Hafner, Timothy Lillicrap, Jimmy Ba, and Mohammad Norouzi. Dream to control: Learning behaviors by latent imagination. In ICLR, 2020.

Danijar Hafner, Timothy Lillicrap, Mohammad Norouzi, and Jimmy Ba. Mastering atari with discrete world models. arXiv preprint arXiv:2010.02193, 2020.

**摘要**

Learned world models summarize an agent's experience to facilitate learning complex behaviors. While learning world models from high-dimensional sensory inputs is becoming feasible through deep learning, there are many potential ways for deriving behaviors from them. We present Dreamer, a reinforcement learning agent that solves long-horizon tasks from images purely by latent imagination. We efficiently learn behaviors by propagating analytic gradients of learned state values back through trajectories imagined in the compact state space of a learned world model. On 20 challenging visual control tasks, Dreamer exceeds existing approaches in data-efficiency, computation time, and final performance.

习得世界模型总结了智能体的经验，以促进学习复杂行为。 虽然通过深度学习从高维感官输入中学习世界模型变得可行，但有许多潜在的方法可以从中推导出行为。 我们展示了 Dreamer，这是一种强化学习智能体，它纯粹通过潜在的想象力从图像中解决长期任务。 我们通过在学习世界模型的压缩状态空间中想象的轨迹将学习状态值的分析梯度传播回来，从而有效地学习行为。 在 20 项具有挑战性的视觉控制任务中，Dreamer 在数据效率、计算时间和最终性能方面超越了现有方法。

Intelligent agents need to generalize from past experience to achieve goals in complex environments. World models facilitate such generalization and allow learning behaviors from imagined outcomes to increase sample-efficiency. While learning world models from image inputs has recently become feasible for some tasks, modeling Atari games accurately enough to derive successful behaviors has remained an open challenge for many years. We introduce DreamerV2, a reinforcement learning agent that learns behaviors purely from predictions in the compact latent space of a powerful world model. The world model uses discrete representations and is trained separately from the policy. Dreamer V2 constitutes the first agent that achieves human-level performance on the Atari benchmark of 55 tasks by learning behaviors inside a separately trained world model. With the same computational budget and wall-clock time, Dreamer V2 reaches 200M frames and exceeds the final performance of the top single-GPU agents IQN and Rainbow.

智能代理需要从过去的经验中进行泛化，以在复杂环境中实现目标。世界模型促进了这种泛化，并允许从想象的结果中学习行为以提高样本效率。虽然最近从图像输入中学习世界模型对于某些任务变得可行，但对 Atari 游戏进行足够准确的建模以得出成功的行为多年来一直是一个开放的挑战。我们介绍了 DreamerV2，这是一种强化学习智能体，它纯粹从强大世界模型的压缩潜在空间的预测中学习行为。世界模型使用离散表示，并与策略分开训练。 Dreamer V2 通过在单独训练的世界模型中学习行为，构成了第一个在 55 个任务中的 Atari 基准测试中实现人类水平性能的代理。在相同的计算预算和时间下，Dreamer V2 达到了 200M 帧，并超过了顶级单 GPU 智能体的 IQN 算法和 Rainbow 算法的最终性能。

**引用**

Hafner et al. proposed Dreamer [hafner2019dream] to learn a world model from past experience and efficiently learn farsighted behaviors in its latent space by backpropagating value estimates back through imageined trajectories.

**动机**

Previously developed model-based agents are typically computationally demanding, or do not fully leverage the learned world model, which is limited in how far ahead they can accurately predict.

**方法**

Dreamer overcomes these limitations by learning a value network and an actor network via backpropagating multi-step value gradients through imagined latent transitions. Specifically, Dreamer leverages the PlaNet [cite] world model, which learns latent dynamic and predicts outcomes based on a sequence of compact model states that are computed from the input images. After that, start imagined trajectory from each latent state of the current data batch. The value model optimizes Bellman consistency for current action model and the action model maximizes values by backpropagating through latent transitions.

**比较**

Experiments show that, compared to model-free methods and previous model-based methods, Dreamer achieves a new state-of-the-art in final performance, data efficiency and computation time.

**与model-free比较**

Compared to model-free methods, world models can interpolate past experience and offer analytic gradients of multi-step returns for efficient policy optimization.

**与World Models比较**

Compared to the World Models, Dreamer is not limited by the fixed imagination horizon and learns long-sighted behaviors from predicted sequences of model states.

**与PlaNet比较**

Compared to the PlaNet, Dreamer side-steps the expensive search for the best action among many predictions by decoupling planning and acting.

**优劣**

The model is shown to have the capability of performing long rollout and value estimation. Moreover, Dreamer learns off-policy and is applicable to both continuous and discrete actions and early episode ends.

**DreamerV2**

Hafner et al. [cite] further proposed DreamerV2 that the vectors of categorical latents enforces learning sparse state representations and KL balancing scales prior vs posterior loss to encourage accurate prior dynamics. In Dreamer and DreamerV2, agents learn behaviors by optimizing the expected values over the predicted latent states. Compared with Dreamer, DreamerV2 replaces the Gaussian latent as proposed in PlaNet [cite] with the discrete latent, which brings superior performance on the Atari 200M benchmark. The possible reason for such effects would be the discrete latent representation can better fit the aggregate posterior and handle multi-modal cases.



## InfoPower

**论文**

Homanga Bharadhwaj, Mohammad Babaeizadeh, Dumitru Erhan, and Sergey Levine. Information prioritization through empowerment in visual model-based RL.

**摘要**

Model-based reinforcement learning (RL) algorithms designed for handling complex visual observations typically learn some sort of latent state representation, either explicitly or implicitly. Standard methods of this sort do not distinguish between functionally relevant aspects of the state and irrelevant distractors, instead aiming to represent all available information equally. We propose a modified objective for model-based RL that, in combination with mutual information maximization, allows us to learn representations and dynamics for visual model-based RL without reconstruction in a way that explicitly prioritizes functionally relevant factors. The key principle behind our design is to integrate a term inspired by variational empowerment into a state-space model based on mutual information. This term prioritizes information that is correlated with action, thus ensuring that functionally relevant factors are captured first. Furthermore, the same empowerment term also promotes faster exploration during the RL process, especially for sparse-reward tasks where the reward signal is insufficient to drive exploration in the early stages of learning. We evaluate the approach on a suite of vision-based robot control tasks with natural video backgrounds, and show that the proposed prioritized information objective outperforms state-of-the-art model based RL approaches with higher sample efficiency and episodic returns.

为处理复杂的视觉观测而设计的基于模型的强化学习 (RL) 算法通常会显式或隐式地学习某种潜在状态表示。这种标准方法不区分状态的功能相关方面和不相关的干扰因素，而是旨在平等地表示所有可用信息。我们为基于模型的 RL 提出了一个修改后的目标，结合**互信息最大化**，允许我们学习基于视觉模型的 RL 的表示和动态，而无需以明确优先考虑功能相关因素的方式进行重建。我们设计背后的关键原则是将受**变分势能**启发的项集成到基于互信息的状态空间模型中。该项优先考虑与行动相关的信息，从而确保首先捕获功能相关的因素。此外，相同的势能项还促进了 RL 过程中的更快探索，特别是对于奖励信号不足以在学习早期阶段推动探索的稀疏奖励任务。我们评估了一套具有自然视频背景的基于视觉的机器人控制任务的方法，并表明所提出的优先信息目标优于基于最新模型的 RL 方法，具有更高的样本效率和episodic回报。

**引用**

InfoPower has been proposed as a visual model-based RL algorithm that integrates empowerment into a mutual information based, non-reconstructive framework, which prioritizes functional-related information from visual observations to learn a more robust representation for state space models.

**动机**

In complex environments with high-dimensional observations, modeling the full observation space can present major computational challenges.

**方法**

InfoPower [cite] has been proposed to integrate empowerment into a mutual information based, non-reconstructive framework, which prioritizes functional-related information from visual observations. Specifically, the empowerment objective prioritizes encoding controllable representations in the latent states, and the forward information objective helps learn a latent forward dynamics model so that future latents can be predicted from the current latent state and the current action. Moreover, the reward objective helps learn a reward prediction model, such that the agent can learn sequence of actions through latent rollouts. And finally, the contrastive learning objective facilitates learning an encoder that maps from image observations to latent states.

**比较**

Compared to previous model-based methods which do not distinguish between functionally relevant aspects of the state and irrelevant distractors, InfoPower prioritizes functional-related information from visual observations and enables more robust representations. Compared to the Dream, experiments show that InfoPower has a better performance in terms of episodic returns in environments with explicit background video distractors, and InfoPower has a better sample efficiency when the reward signal is weak. Moreover, the learned latent representations of InfoPower is very similar to the underlying simulator states.



## Open Issues

### Computational Efficiency

Visual model-based RL often requires additional computation, both for training the model and for planning the operation itself. For the model-based methods mentioned above, splitting the agent into a large-world model and a small controller model enables less computation without sacrificing the capacity and expressiveness of the large-world model. Furthermore, planning can improve performance by increasing the computational budget of search actions. However, how to improve computational efficiency while maintaining model performance remains a grand challenge worthy of attention.

### Generalization

Learned dynamics can be independent of any specific task, and visual MBRL can place agents in different environments, infer tasks from visual observations, and implement transfer between various tasks and multi-tasks control with as little hyperparameter changes as possible. Meta RL and transfer RL help to generalize the model, turning the optimization of a single task into a more general artificial intelligence. Among them, meta RL relies on the randomization of models to generate meta-policies that can generalize to similar environments and adapt to dynamic tasks. However, how to generate models to adapt the trained meta-policies to the target environment remains a huge challenge.

### Hierarchy

State abstraction [Dietterich, 1999] and temporal abstraction [Sutton et al., 1999] can map raw MDPs to low-dimensional and compact MDPs, greatly simplifying the task of RL. Hierarchical RL (Barto and Mahadevan, 2003) can be approached in a model-based manner, where a higher-level action space defines a model with temporal abstraction. Hierarchical RL can theoretically reduce the number of samples (Brunskill and Li, 2014) and computational complexity (Mann and Mannor, 2014) for solving MDPs, and provide PlaNet with learning of temporal abstractions instead of fixed action repeat. We have observed some possibilities for learning abstract models [Jiang et al., 2015, Zhu et al., 2022b], however, much work remains to be done.

### Video prediction

Video prediction is an active research area related to visual MBRL, which trains a model to take the current image frame of a video sequence to predict the next frame. In the field of robotics, video predictive models for planning [54, 55, 56] focus on handling the visual complexity of the real world and solving tasks using robotic arms. In contrast, visual MBRL focuses on simulating the environment, exploiting latent planning to expand to larger state and action spaces, longer planning horizons, and sparse reward tasks.

### Multiagent RL

Planning in various multi-agent scenarios with the multi-agent environment model is still in its infancy. As noted in a recent survey of model-based MARL, research in this direction is just beginning, with only a few established works. Looking at recent emerging work, incorporating model-based approaches to improve coordination among team agents and increase the sample efficiency of training has great potential. Potential topics for future development of model-based MARL include improving the scalability of centralized approaches, as well as learning model-based decentralized approaches and new designs of communication protocols.



## Research Plan

My research interests are theory and applications of machine learning and reinforcement learning. My research plan is to do research in the frontier areas of reinforcement learning such as Model-based RL and offline RL under the guidance and advice of my supervisor.

Offline RL provides the possibility of transforming environment-interactive RL into data-driven RL, which helps to improve the performance of RL training by using the currently widely used supervised learning ideas, avoiding the problem of low sampling efficiency in traditional RL. In the field of offline RL, Model-based is a class of methods that are difficult to learn but easy to explore and evaluate policies. Model-based offline RL exploits the generalization ability of the model to perform a certain degree of exploration and generates additional training data to improve policy performance. However, since offline data is usually limited, the learned model is considered untrustworthy, and most methods adopt a conservative strategy. There are some methods that use adaptive strategies to model out-of-distribution actions, which provides a direction for solving the problem of environment models that are difficult to learn. In addition, both meta-learning and transfer learning have the characteristics of acquiring historical experience and adapting to new tasks, which can both provide help for learning environmental models.

Overall, I think Model-based RL and offline RL are research directions worth looking forward to and exploring.
