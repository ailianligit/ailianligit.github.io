# Pruning in ML & FL

## 【机器学习】

- ### **(ICLR 2017) Pruning Filters for Efficient ConvNets**

  - **背景**：以往的模型压缩主要修剪了全连接层的参数，但是这部分的计算量很低。例如，VGG-16中的全连接层参数占了90%，而只影响1%的浮点计算量。卷积层也可以修剪，但是需要稀疏BLAS库甚至专门的硬件资源来进行计算加速。
  - **Insights**：不同层对剪枝的敏感度不同，坡度越缓越敏感
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=OTExOGUxNmJkOTAzNGZjODZmNzNjNWIyMDU1ZGMyNzlfM2FnRnNURVlONURodkJoTVFQVXB3NGk4d2RwdzJKdzhfVG9rZW46Ym94Y24zRjE2Q2c3TWRHdDdZMDFKUDN2aVBmXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)
  - **贡献**： 提出了一种one-shot filter pruning策略（structural pruning），根据filter的$$$$l_1 $$$$范数大小确定重要度，同时考虑层与层之间剪枝的关系，最后经验性的确定每层的剪枝率（划分不同stage，每个stage包含连续若干层，stage越大剪枝率越低），并在新剪枝的模型上进行重训练以提升模型效果。
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=YjM2ZmU2NjY5Y2NhYWQ4NWEwYjQ5OWVlMzFlNzAzMTFfVklnVWtSZ2pCcE56YUFBTXVQODU3c25wYVBGTkI0NURfVG9rZW46Ym94Y25uNjFhcmd1M0J3UFBVbmt6UHdMSk1jXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)
  - 启发：在联邦学习自适应剪枝中区别对待不同层的参数



- ### **(IJCAI 2018) Accelerating** **Convolutional** **Networks via Global & Dynamic Filter Pruning**

  - **背景**：CNN的训练和推理依赖大量的计算资源，特别是随着CNN网络增大，会导致较高的延迟。传统上CNN计算会采用以下几种方法：
    - Low-rank decomposition: 将filters分解为一些参数较少的tensor序列
    - Parameter quantization: 采用参数乘积量化来减少参数空间的冗余
    - Network pruning
      - Non-structured: 独立地在每一层修剪多余的参数，会导致不规则的内存访问，影响推理效率，往往需要额外的软硬件支持，来加速推理过程
      - Structured: 直接修剪filter（filter-wise or channel-wise）
  - **问题**：filter pruning作为一种非常有前景的的方案，可以降低模型的内存使用以及加速训练过程。但是，大部分方式是以layer-wise固定的方式修剪filters，一旦剪错，就无法恢复之前的filters。
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=YWMzMmJkNzM2MDA1NWNhMzQzMzYxNWJlNTAyN2MzYTFfbnFGWGkxRHFtMldsV21GT1h1SklXUkFPdmU1ZzFaelBfVG9rZW46Ym94Y25KZFZpWkZVY3JWb0xoMExrZU5WM1piXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  -  （彩色方块代表filter，组成一个filter set，即$$$$W^* $$$$；黑色/白色方块代表对应的filter是重要/非重要，即$$$$m $$$$）

  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTQ1MTJlYjYyMzBiNzY2M2MzMmFjZTJhNzA1ZmU0NjRfWlpiajhrZG5IZXRuSnNvbWdFczAyTmFsd3JRNXJkbnhfVG9rZW46Ym94Y25UQVdYdDNJdkVZampRQWRPZWZyWEVTXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 提出的优化问题：$$$$h(\cdot) $$$$是全局区分函数，计算filters的显著值，并决定global mask；$$$$\beta $$$$用于决定网络的叙述程度。由于$$$$\parallel \parallel_0 $$$$算子的引入导致问题是NP-hard问题。
  - 简化求解方法：交替更新$$$$W^* $$$$和$$$$m $$$$，前者使用梯度下降，后者贪心选择最重要的filters
  - **贡献**：提出一个全局动态剪枝算法（GDP），跨层减去冗余的filters，同时可以动态地更新filter saliency，并且恢复被错误修剪的filters，进而重新训练以提高模型准确率。



- ### **(NeurIPS 2021) Validating the Lottery Ticket Hypothesis with Inertial Manifold Theory**



## 【联邦学习】

关键字（federated + pruning）

- ### **(ICDE 2022) FedMP: Federated Learning through Adaptive Model Pruning in Heterogeneous Edge Computing**

  - **问题**：一方面，边缘节点通信和计算资源有限，大模型传输和训练面临挑战；另一方面，节点异构性强，低性能节点会严重拖慢高性能节点的学习效率。
  - 剪枝策略：structured pruning，使用$$$$l_1 $$$$范数评估filter/neuro的重要性，设置剪枝率参数在每一层进行剪枝。
    - unstructured pruning：生成sparse-model，很难加速训练，除非有专门的库和硬件
    - structured pruning：生成sub-model，可以同时降低训练和通信时间  
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=N2E3MDdhZjI2MjU1M2JmOTFjOTM4MGEwODIzYzU3MmJfcTNUM2I0UDVnZGd6SlpHNWNaa3FoUDJJMlNPY2Q1UEhfVG9rZW46Ym94Y25JeTRzOUd1MU5NTHJiSm9HZzlOZVlkXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 动态剪枝：采用多臂老虎机方法最大化以下奖赏（考虑客户端的耗时差异和模型效用）：
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=OGUzNzJlNDBmNjZjNzJiOTIxNGIwZjRhMzkwM2QwYzJfd3lmT3FCSzJ2Y2FiZnZ5cVpOd1ppTGg4cGtONGZ0WWJfVG9rZW46Ym94Y25XdmQxTEpTOE5vYlR6Z2NDdWF6WjhlXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - **贡献**：提出FedMP算法，将模型剪枝应用于联邦学习，证明了**理论收敛率**。为了适应异构受限的边缘节点，使用Multi-Armed Bandit根据每一个节点资源情况**动态决定剪枝率**，每个节点将会收到适合它的sub-model，并提出了**基于模型残差的聚合策略**。
  - **改进**：
    - 每一层的剪枝率都设为一样，可能不是最优方法
    - 奖赏函数设计有些问题，可能一味为了缩短客户端之间的时间差距，而没有得到较好的剪枝效果



- ### **(IJCAI 2022) FedDUAP: Federated Learning with Dynamic Update and Adaptive Pruning Using Shared Data on the Server.** 

  - **问题**：
    - 客户端本地数据量有限，本地训练有效性差
    - 客户端通信和计算资源有限
  - 前提：服务器有非敏感的训练数据和客户端上传本地元数据统计信息（比如label分布）不会引起太多隐私问题。
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=YTMwN2JkOTVmNTg2NmVlMTk4OGY1MDg1MGUyMzc3MGNfdFVaUW5FM1M3UFBmeDE3aXh3TzljcUNFT01mSWRCQTNfVG9rZW46Ym94Y25YblZTMGw2WHI5WjBsak1aUkthTjRkXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 方法：
    - 服务器动态更新：使用设备和服务器数据动态更新模型，服务器根据全局模型准确率和non-IID程度自适应调整中心训练。具体地，在客户端聚合参数的基础上利用本地数据进一步更新模型（模型准确率越$$$$acc^t $$$$高贡献越小，数据量$$$$n_0 $$$$越小贡献越小，数据差异越大贡献越小）
    - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=MjIyMTMxZDgyN2Y2OTRmZGI0MWRmNGY0NDRlMTU5ODVfWHFtYmFGVmdEcGtRUXJMWDFBWUVkb0FCRjM2WEc5MEtfVG9rZW46Ym94Y25CckFadUx5QVpTV24xSlQzMFdsQW1jXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

    - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=MTk0YzZjNjNkMDAxNDlkYTZjNjEzYjQ3NzJhOTEyMDJfWlc3azUyQkZkdDBLbFh4d3F5bWFTMVQ3MTBvTXJlZ3BfVG9rZW46Ym94Y25aMUdTUWZZZHFVNFhTTDdnYWZ6dUJlXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

    - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjE3NzgyZjI1YWMwMjlhNDk2OGI5NDNkMTc0MDJmMTVfc1BrRmRqVDZWMktYR0RzMGlXMGdDNTcxTlh1Vk5qc3NfVG9rZW46Ym94Y24yc0lGb05FeFdXaUNqanRJOVBsTzlkXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

    - 服务器剪枝策略：structured pruning，在提升训练效率并保证comparable模型准确率。具体地，每个客户端根据loss函数的海森矩阵计算剪枝率（引用NIPS一篇利用惯性流体理论），送到服务器聚合后（如下面公式），再根据每个卷积层输出feature maps的rank进行剪枝
    - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=MWQ1YWU1OTU1NjAxMWJkNmE0ZTc5NTJhMGI2ODBlNzNfcFppYVM5MG9JclpvM1ZDUnc0eVRIT2VPYVFacEQybVBfVG9rZW46Ym94Y25ZZURjZUVrWHY5SXFheHpOZzBaU2ZiXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)
  - 改进：
    - 像是一个A+B工作，缺乏理论贡献，将原来的one-time pruning拓展成adaptive pruning
    - 如何在剪枝的过程中考虑non-iid的影响？



- ### **(TNNLS 2022) Model Pruning Enables Efficient Federated Learning on Edge Devices.**

  - **代码**：https://github.com/jiangyuang/PruneFL
  - **问题**： 考虑到客户端的有限资源（计算、通信、内存），如何让FL在合理的时间/能耗下高效的训练模型？
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjRkYmIyMjI3MTAyMTM5NTI1NzY3OWZlNjE3YWVjNWRfdkVhTXRrNFJXTENDSzZWRnkzaEFCYjNjRFVUc2dqdVpfVG9rZW46Ym94Y250SFljTVBSSFI0ck9UbG5neGhQTTJnXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - **方法**
    - 初始剪枝：选择一个强节点进行初始剪枝，使得FL一开始训练的模型就比较小
    - 进一步剪枝：在FL的过程中，动态剪枝以进一步降低模型大小
    - 自适应剪枝：*unstructured pruning*，单位时间经验损失降低最大化
    - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=MjFhOGM0MDBmMmZlNGY2NTJiN2U5ZThiOTQ1MTY0Y2JfSGE1a0prU0tNMVRkTlM0T1p2d2lWYW50WXF3Qm9yTFVfVG9rZW46Ym94Y24ySVJ1QmdOVWR2VTVZVldOUWgyNzBjXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

    - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=N2RjYzY3MDlhMTJiNjg4YTFjMjBjY2Q5MTlmOWM1NThfbzJsalU1aE95bUR5cFZGSXF0ZUtTRzEwdkpNdTRmVm5fVG9rZW46Ym94Y241bm5HU2RwS3prOWJ3UEV6cHhORVFnXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

    - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmJlNzc2Y2M4YzMwYjQ5ODM2NjYxNzJkZWUyZjc2ZTBfOWh4YkhUOFVGNUo2b05GTGR6RUNielZiYUV2SVluY3hfVG9rZW46Ym94Y241UjRwVTN5TjZRQXM4djhuN2RoNUFoXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

    - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=NTUxZTM2ODI3MGVjNzdkYzIyMjZiOWQ2NjE1MDhlNjdfZlZ2dTdiUUNWbVg0SmVGUUthRVk5bU9mZDhXRUw2aWlfVG9rZW46Ym94Y25iWm1sa3BoVTNlY3U4UXFMaGE1VlAwXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

    - **贡献**：提出了two-stage剪枝策略，提出**经验误差近似方法**，并通过最大化单位时间经验误差**动态选择剪枝参数**，并给出了**收敛率证明**。此外，还**扩展了Pytorch库**以实现更加高效的稀疏存储和稀疏卷积。
    - 启发：有区别地对待不同层的剪枝策略，本文考虑了不同层参数对时间的影响，还可以考虑对模型准确率的影响



- ### **(**Mobicom **2022)** **NestFL: efficient federated learning through progressive model pruning in heterogeneous edge computing**

  - It is the same as **(TNNLS 2022) Model Pruning Enables Efficient Federated Learning on Edge Devices**



- ### **(Trans. Commun. 2022) Joint Model Pruning and Device Selection for Communication-Efficient Federated Edge Learning**

  - 问题：在无线联邦学习中，除了传统FL中面临的客户端计算资源有限和异构的问题外，还面临着有限带宽资源和不稳定的无线信道等问题。
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=MTY2OTU1ODI1Y2FjMGFjYTMxZDZmNDFmMWFkMWI3NGZfTGNTenpHR25QZnBONW4yZnRWNjJzUE8wQk14MU9aV1VfVG9rZW46Ym94Y25MYmd5NDdFZGZPMlh6NVlyMXh4Q0hiXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 贡献：为了在无线联邦学习中，让通信更加高效，作者综合使用了剪枝、客户端选择和无线资源分配的手段。首先，对所提出的联邦剪枝算法进行了收敛分析；然后提出了一个最优化问题（通过动态控制剪枝率、客户端端选择和无线资源分配，在有限时间内最大化收敛率）；最后推导出了最优剪枝率和资源分配的解析解，并提出了一个threshold-based客户端选择算法进一步提高学习效率。



- ### **(Wirel.Commu. Letters 2021)** **Adaptive Network Pruning for Wireless Federated Learning**

  - It is the same as **(Trans. Commun. 2022) Joint Model Pruning and Device Selection for Communication-Efficient Federated Edge Learning**



- ### **(IOTJ 2022) IoT Device Friendly and Communication-Efficient Federated Learning via Joint Model Pruning and Quantization**

  - 背景：随着DNN模型的增大，直接将联邦学习运用到物联网中会面临各种问题。这主要因为物联网设备计算和存储资源受限，无法直接存储较大的模型，在大模型上训练和推理都会带来较大延迟。
  - 问题：在IoT场景下如何高效的实现FL？克服计算、存储和通信的限制。
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTAwMzcxMzUyOThjNTdjNWFmMjk2MGQ3MWJmOTIyYzNfY0NQSUp5bTBxSHNzSTBzbGtlUFVTTzhQVzlzUGY0RmxfVG9rZW46Ym94Y25jajVFVnhNSktyT1VTVDVhRUNmOFd6XzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 贡献：综合使用**双量化**和**模型剪枝**的技术实现高效联邦学习。双量化指在服务器上对聚合参数进行量化降低服务器广播全局模型的带宽开销，客户端在本地对训练的梯度进一步量化以降低本地上传模型的开销（还集成了**错误反馈机制**）。剪枝在客户端实现，通过设定最终简剪枝率实现递进式剪枝。文中进一步对这种综合策略的**收敛率**进行了分析。
  - 改进：超参数过多，例如最终剪枝率和量化程度都是提前设定的，与模型和数据集以来较大。



- ### **(CoRR 2022) On the Convergence of Heterogeneous Federated Learning with Arbitrary Adaptive Online Model Pruning**

  - 代码： https://github.com/hanhanAnderson/FL_Converge
  - 问题：目前，学术界已经提出了许多联邦剪枝的策略，包括固定结构（HeteroFL）、基于彩票假设、自适应结构（PruneFL），但是相关的理论还不够健全和统一
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=NDM0YTE0ZjM0ODg2MjRlMDYzZTdiNDdhZWJkNjUyMjdfUnVkVXFUQUxNcE45Q25yQ29CNGlGUlpyRFJuUjRyRllfVG9rZW46Ym94Y25aWkFDeFE0bDR0MFBDY0NMWFdkNThnXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 贡献：本文提出一种统一的联邦剪枝框架，并提出了除剪枝率外另一个指标——最小覆盖指数（把模型划分互不相交的k个子区域，子区域覆盖clients的最小值），并证明的收敛性。如上图，（a）只考虑剪枝率，（b）(c)同时考虑最小覆盖指数。
  - 改进：理论证明比较细致（10多页）和实验很细节（10多页），但实用性有限，也没有考虑客户端异构的最优剪枝策略设计。



- ### **(CIKM 2022) Neuron Specific Pruning for Communication Efficient Federated Learning**

  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=M2ZjMGM1MGZkMjI5MzM0MjZhZDBmZGMzODNiNmZjNWNfYU1WemYzd1RjZHNiclp4a1NDaVk4QnlBbzBvb1hTQTBfVG9rZW46Ym94Y252elh0QzdRZWRIY2MzdklMTEpkcFZjXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTI0NTIwNTRjYzZjYTk1NDBjYzNiOGE5NDMzYWQyMThfUkczZW1sOHZVNURpbGpQOTJiT0ltMUhkTnN4NURTcmdfVG9rZW46Ym94Y252S05tT3EzSU9BTWR6SXBSbENGZHJlXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 

  - 剪枝策略：NISP (Neuron Importance Scope Propagation)，根据neuro的重要度分数减去排名考核后的，属于structured pruning。来自CVPR 2018，思想上是为了让剪枝前后的网络模型输出尽可能小。
  - 贡献：将NISP直接用到FL中，每个client对应相同的剪枝mask，为了解决FL场景下缺少最后一层输出参数的问题，本文只从倒数第二层开始剪枝。



- ### **(ICDCS Worshops 2021) Personalized Federated Learning by Structured and Unstructured Pruning under Data Heterogeneity**

  - 代码：https://github.com/MMorafah/Sub-FedAvg
  - 背景：联邦学习中由于数据分布的异构性，一个全局模型很难适应不同客户端的学习需求，因此需要为不同的客户端定制个性化模型。
  - 贡献：将混合剪枝技术应用到联邦学习中，为不同client提供不同的subnetwork。将structued剪枝应用于卷积层，将unstructed剪枝应用于全连接层。服务器收到clients的模型后只在模型参数交集中进行聚合。



- ### **(WWW 2022) Federated Unlearning via Class-Discriminative Pruning**

  - 背景：由于客户端硬件能力有限和隐私需求的原因，往往需要在训练好的FL模型中去掉一些特殊类标签的信息，这称为联邦反学习。传统的机器反学习技术应用在集中化的场景，假设可以获取所有数据，并没有考虑non-iid下的适配。而重新学习又会带来较大的时间和计算开销。
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=NGJkY2Q2ZDFjZWFmZTNmYmQyY2NhZGMyYTFmMjc5MTdfZVJkVkY2aTJ2WmdFR0FFS3U0dm96SnJRZEtIcW4zY2JfVG9rZW46Ym94Y25Ocm81UkJoUDg2YnEyTjA4emM5ZFozXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 贡献：本文提出了一种基于TF-IDF剪枝的方法，可以修剪模型中与去除分类中最相关的filters，从而达到反学习的效果。
  - 方法：作者通过实验发现对于不同的图片，feature maps会有较强的关联。例如第二行（52-th channel）主要识别头部信息，第三行（127-th channel）主要识别文字信息。量化feature maps和class的关联关系使用了NLP中常用的TF-IDF指标，表示word（feature map）与document（class）关联关系，分数越高，表明word（feature map）出现在document（class）的概率越大，所以优先减去这类filters。
  - ![img](https://xxtedtwvt0.feishu.cn/space/api/box/stream/download/asynccode/?code=NTRlNjY5YTc4ZDdlZGRmY2E4ZDU5YmVlMzkyYTkzMjlfQzBBektnOWhKVGJBVkxSOW10dHdER1Q4R2txYllrdEdfVG9rZW46Ym94Y25vQUZGQ05vY2RBckFRbXZSYlpvUlZmXzE2Njk2MzgzNjI6MTY2OTY0MTk2Ml9WNA)

  - 启发：目前的联邦剪枝算法主要考虑整体准确率和效率的平衡，主要根据weights的大小修剪filters，是否可以从标签长尾分布的角度考虑，保证一些与filters优先保留，这些filters对数据量较少的重要类别标签的分类作用较大。



- ### **Towards Sparsified Federated Neuroimaging Models via Weight Pruning.** [DeCaF/FAIR@MICCAI 2022](https://dblp.uni-trier.de/db/conf/miccai/decaf2022.html#StripelisGDSTA22): 141-151

- ### **Efficient Federated Tumor Segmentation via Normalized Tensor Aggregation and Client Pruning.** [BrainLes@MICCAI (2) 2021](https://dblp.uni-trier.de/db/conf/brainles-ws/brainles-ws2021-2.html#YinYLJCDH21): 433-443

- ### **Federated Pruning: Improving Neural Network Efficiency with Federated Learning.** [INTERSPEECH 2022](https://dblp.uni-trier.de/db/conf/interspeech/interspeech2022.html#LinXYZ0MB22): 1701-1705

- ### **Fourier Enhanced MLP with Adaptive Model Pruning for Efficient Federated Recommendation.** [KSEM (3) 2022](https://dblp.uni-trier.de/db/conf/ksem/ksem2022-3.html#AiWLWC22): 356-368

- ### **Neural Network Compression and Acceleration by Federated Pruning.** [ICA3PP (2) 2020](https://dblp.uni-trier.de/db/conf/ica3pp/ica3pp2020-2.html#PeiWQ20): 173-183



## 【总结和未来工作】

| Item                 | Publication                                  | Constrained Resource | System Heterogeneity | Data Heterogeneity | Privacy | Theoretic analysis | Open Source | How to solve                                                 |
| -------------------- | -------------------------------------------- | -------------------- | -------------------- | ------------------ | ------- | ------------------ | ----------- | ------------------------------------------------------------ |
| FedMP                | ICDE 2022                                    | ✔                    | ✔                    | ❌                  | ❌       | ✔                  | ❌           | Structured + Magnitude + MAB                                 |
| FedDUAP              | IJCAI 2022                                   | ✔                    | ❌                    | ❌                  | ❌       | ❌                  | ❌           | Structured + Server update + Inertial Manifold Theory        |
| PruneFL              | TNNLS 2022 Mobicom 2022                      | ✔                    | ❌                    | ❌                  | ❌       | ✔                  | ✔           | Unstructured + Magnitude + Two stage + Heuristic             |
| Optimal Pruning      | Trans. Commun. 2022Wirel.Commu. Letters 2021 | ✔                    | ✔                    | ❌                  | ❌       | ✔                  | ❌           | Unstructured + Magnitude + Wireless + Biconvex optimization  |
| GWEP                 | IOTJ 2022                                    | ✔                    | ❌                    | ❌                  | ❌       | ✔                  | ❌           | Unstructured + Magnitude + Gradual pruning +Quantization + Error feedback |
| Unifying framework   | CoRR 2022                                    | ✔                    | ❌                    | ❌                  | ❌       | ✔                  | ✔           | (Un)structured + Magnitude + Minimum Covering index          |
| FedSparse            | CIKM 2022                                    | ✔                    | ❌                    | ❌                  | ❌       | ❌                  | ❌           | Structured + Magnitude + NISP                                |
| SubFedAvg            | ICDCSW 2021                                  | ✔                    | ❌                    | ✔                  | ❌       | ❌                  | ✔           | Hybrid pruning + Magnitude + Personalization                 |
| Federated Unlearning | WWW 2022                                     | ✔                    | ❌                    | ❌                  | ✔       | ❌                  | ❌           | Pruning + TFIDF                                              |

- **当前工作** 
  - 联邦剪枝主要围绕**剪什么**，**剪到什么程度**？
    - 预先设定：GWEP、Unifying framework、FedSparse、Federated Unlearning
    - 自适应：FedMP、FedDUAP、Optimal Pruning
    - 混合：PruneFL、SubFedAvg
  - 直接将现有剪枝策略套到FL中：
    - FedDUAP：Validating the Lottery Ticket Hypothesis with Inertial Manifold Theory. [NeurIPS 2021](https://dblp.uni-trier.de/db/conf/nips/neurips2021.html#ZhangJZZZRLWJD21): 30196-30210
    - FedSparse：NISP: Pruning Networks Using Neuron Importance Score Propagation. [CVPR 2018](https://dblp.uni-trier.de/db/conf/cvpr/cvpr2018.html#Yu00LMHGLD18): 9194-9203

- **未来工作**
  - **数据异构**的联邦剪枝：目前联邦剪枝大部分直接借用机器学习的剪枝算法（magnitude-based/one-shot/iterative/adaptive pruning），有几篇会考虑联邦学习系统异构特性，为不同客户端设置不同的剪枝率，但还没有看到建立数据non-iid程度与剪枝策略关系的论文
  - **长尾分布**的联邦剪枝：大部分联邦剪枝所设计的参数重要度评估函数有利于大部分的数据，而某些标签的数据量较少可能很重要，在剪枝过程中因为参与更新全局模型的比重较低很可能会被优先剪去，一个可行的方向是重新设计模型参数的filter/neuro重要度评估函数
  - 联邦剪枝**系统**：目前找到联邦剪枝的开源代码有3个，暂时还没有比较全面、系统的开源代码
  - 基于Switch的剪枝算法设计：动态剪枝