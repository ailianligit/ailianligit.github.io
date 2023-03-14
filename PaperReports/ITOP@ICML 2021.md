# [ITOP@ICML 2021] Do We Actually Need Dense Over-Parameterization? In-Time Over-Parameterization in Sparse Training

## Abstract

在本文中，我们通过在稀疏训练中提出实时过参数化 (ITOP) 的概念，引入了一种新的视角来训练能够获得最先进性能的深度神经网络，而无需进行昂贵的过参数化。 **通过从随机稀疏网络开始并在训练过程中不断探索稀疏连接性，我们可以在训练过程中执行过参数化，缩小稀疏训练和密集训练之间的可表达性差距。** 我们进一步使用 ITOP 来了解动态稀疏训练 (DST) 的底层机制，**并发现 DST 的好处来自于它在搜索最佳稀疏连接时跨时间考虑所有可能参数的能力**。 只要可靠地探索了足够的参数，DST 就可以大大优于密集神经网络。 我们提出了一系列实验来支持我们的猜想，并在 ImageNet 上使用 ResNet-50 实现最先进的稀疏训练性能。 更令人印象深刻的是，ITOP 在极端稀疏情况下取得了优于基于过参数化的稀疏方法的优势性能。 当在 CIFAR-100 上使用 ResNet-34 进行训练时，ITOP 可以在 98% 的极端稀疏度下匹配密集模型的性能。



## Introduction

尽管事实上训练目标函数通常是非凸的和非光滑的（Goodfellow et al., 2015; Brutzkus et al., 2017；Li & Liang，2018；Safran & Shamir，2018；Soudry & Carmon，2016；Allen-Zhu 等，2019；Du 等，2019；Zou 等，2020；Zou & Gu，2019）。 同时，先进的深度模型（Simonyan & Zisserman，2014；He 等人，2016；Devlin 等人，2018；Brown 等人，2020；Dosovitskiy 等人，2021）不断实现最先进的水平 结果导致许多机器学习任务。 在实现令人印象深刻的性能的同时，最先进模型的尺寸也在爆炸式增长。 训练和部署那些高度过度参数化的模型所需的资源令人望而却步。

出于推理的动机，大量研究（Mozer & Smolensky，1989 年；Han 等人，2015 年）试图发现一种稀疏模型，该模型可以充分匹配相应密集模型的性能，同时大幅减少参数数量。 这些技术虽然有效，但涉及对高度过度参数化的模型进行至少一个完整的收敛训练时间（完全密集的过度参数化）的预训练（Janowsky，1989 年；LeCun 等人，1990 年；Hassibi 和 Stork，1993 年；Molchanov 等人 ., 2017; Han et al., 2016; Gomez et al., 2019; Dai et al., 2018a) 或部分收敛训练时间（部分密集过参数化）（Louizos et al., 2017; Zhu & Gupta, 2017 年；Gale 等人，2019 年；Savarese 等人，2019 年；Kusupati 等人，2020 年；You 等人，2019 年）。

鉴于最先进的模型（例如 GPT-3（Brown 等人，2020 年）和 Vision Transformer（Dosovitskiy 等人，2020 年））的训练成本越来越大，这种严重过度参数化 依赖性导致大多数机器学习社区无法获得最先进的模型。

最近，彩票假设 (LTH) (Frankle & Carbin, 2019) 显示了从头开始训练子网络（稀疏训练）以匹配密集网络性能的可能性。 然而，这些“中奖彩票”是在完全密集的超参数化过程（迭代修剪完全融合网络）的指导下找到的，以及通过部分密集超参数化（初始化时修剪（Lee 等人， 2019；Wang 等人，2020；de Jorge 等人，2020)）或没有过度参数化（随机初始化的静态稀疏训练（Mocanu 等人，2016；Evci 等人，2019）））通常无法 以匹配其密集对应物所达到的准确性。 一个常识性的解释是，与密集训练相比，稀疏训练，尤其是在极高稀疏度下，不具有过参数化特性，因此可表达性较差。 解决这个问题的一种方法是利用从密集训练中学到的知识，例如 LTH（Frankle & Carbin，2019）。 虽然有效，但过度参数化密集训练所附带的计算成本和内存要求令人望而却步。

### Our Contribution

在本文中，我们提出了一个称为 InTime 过参数化 1 的概念，以缩小过参数化方面的差距以及稀疏训练和密集训练之间的可表达性，如图 1 所示。**不是从密集和预训练模型继承权重，而是允许 跨训练时间的连续参数探索在时空流形中执行过参数化，这可以显着提高稀疏训练的可表达性。**

我们发现实时过参数化的概念有助于 (1) **探索稀疏训练的可表达性，特别是对于极端稀疏性**，(2) **降低训练和推理成本** (3) 理解动态稀疏训练的潜在机制 ( DST) (Mocanu et al., 2018; Evci et al., 2020a), (4) **在防止过度拟合和提高泛化方面**。

基于实时过参数化，我们在 ImageNet 上使用 ResNet50 提高了最先进的稀疏训练性能。 我们通过将 ITOP 概念应用于稀疏训练方法的主要类别 DST，与基于过参数化的稀疏方法（包括 LTH、渐进幅度剪枝 (GMP) 和初始化剪枝 (PI)）进行比较，进一步评估了 ITOP 概念。 我们的结果表明，当达到充分且可靠的参数探索时（根据 ITOP 的要求），DST 始终优于那些基于过度参数化的方法。 由于 ITOP 在整个训练过程中消除了密集的过参数化，因此它可以以更少的训练 FLOPs 匹配相应的密集网络的性能，如图 2 所示。



## Related Work

### Dense Over-Parameterization

依赖于密集过度参数化（密集到稀疏训练）的稀疏诱导技术已被广泛研究。 我们根据它们对密集超参数化的依赖程度将它们分为三类。

**全密集过参数化。** 寻求从完全预训练的密集模型继承权重的技术历史悠久，由 Janowsky (1989) 和 Mozer & Smolensky (1989) 首次引入，作为迭代修剪和保留方法自主发展。 迭代剪枝和保留的基本思想包括三个步骤：（1）完全预训练密集模型直到收敛，（2）剪枝对性能影响最小的权重或神经元，以及（3） 重新训练修剪后的模型以进一步提高性能。 修剪和再训练周期至少需要一次 (Liu et al., 2019)，通常需要多次 (Han et al., 2016; Guo et al., 2016; Frankle & Carbin, 2019)。 用于修剪的标准包括但不限于大小 (Mozer & Smolensky, 1989; Han et al., 2016; Guo et al., 2016)、Hessian (LeCun et al., 1990; Hassibi & Stork, 1993)、 互信息 (Dai et al., 2018a)，泰勒展开 (Molchanov et al., 2016; 2019)。 除剪枝外，其他技术包括变分 dropout（Molchanov 等人，2017 年）、目标 dropout（Gomez 等人，2019 年）、强化学习（Lin 等人，2017 年）也从预训练的密集模型中生成稀疏模型 模型。

**部分密集过参数化。** 另一类方法从密集网络开始，并在训练过程中不断地稀疏化模型。 Gradual magnitude pruning (GMP) (Narang et al., 2017; Zhu & Gupta, 2017; Gale et al., 2019) 被提议通过在 培训课程。 Louizos 等人有一些例子。 (2017) 和 Wen 等人。 (2016) 利用 L0 和 L1 正则化通过显式惩罚不同于零的参数来逐渐学习稀疏性。 最近，Srinivas 等人。 (2017); 刘等人。 (2020a)； 萨瓦雷斯等人。 (2019); 肖等。 (2019); 库苏帕蒂等人。 (2020); 周等。 (2021) 通过引入可训练掩码以在训练过程中学习理想的稀疏连接，进一步推进。 由于这些技术是从密集模型开始的，因此训练成本比训练密集网络要小，具体取决于学习最终稀疏模型的阶段。

**一次性密集过参数化。** 最近，出现了初始化修剪 (PI) 的工作（Lee 等人，2019 年；2020 年；Wang 等人，2020 年；Tanaka 等人，2020 年；de Jorge 等人，2021 年）以获得可训练的稀疏神经网络 基于一些显着性标准的主要训练过程之前的网络。 这些方法属于密集过参数化的范畴，主要是因为密集模型需要训练至少一次迭代才能获得那些可训练的稀疏网络。

### In-Time Over-Parameterization

动态稀疏训练。 与 LTH 并行发展的 DST 是一类不断增长的方法，用于在整个训练过程中使用固定参数计数从头开始训练稀疏网络（稀疏到稀疏训练）。 该范例从（随机）稀疏神经网络开始，并允许稀疏连接在训练期间动态演变。 它首先在 Mocanu (2017) 中引入，并在 Mocanu 等人中得到完善。 **(2018) 通过提出稀疏进化训练 (SET) 算法，该算法比静态稀疏神经网络具有更好的性能。** 除了适当的分类性能外，它还有助于检测重要的输入特征（Atashgahi 等人，2020 年）。 贝莱克等人。 (2018) 提出深度重新布线，通过从后验分布中采样稀疏配置和权重来训练具有严格连接约束的稀疏神经网络。 后续工作进一步引入了权重再分配（Mostafa & Wang, 2019; Dettmers & Zettlemoyer, 2019; Liu et al., 2021）、**基于梯度的权重增长（Dettmers & Zettlemoyer, 2019; Evci et al., 2020a）**，以及 反向传递中的额外权重更新（Raihan & Aamodt，2020；Jayakumar 等人，2020）以提高稀疏训练性能。 通过放宽固定参数计数的约束，Dai 等人。 (2019; 2018b) 提出了一种基于基于梯度的增长和基于幅度的修剪的增长和修剪策略，以产生准确但非常紧凑的稀疏网络。 最近，刘等人。 (2020b) 首次说明了使用动态稀疏训练的真正潜力。 通过开发一个独立的框架，他们可以在典型的笔记本电脑上训练真正的稀疏神经网络，而无需掩码，神经元数超过一百万。

了解动态稀疏训练。 同时，一些作品试图理解动态稀疏训练。 刘等人。 (2020c) 发现 DST 逐渐将初始稀疏拓扑优化为一个完全不同的拓扑。 尽管存在许多可以实现类似损失的低损失稀疏解决方案，但它们在拓扑空间上有很大不同。 Evci 等人。 (2020b) 发现由密集初始化初始化的稀疏神经网络，例如，He 等人。 (2015)，梯度流较差，而 DST 可以显着改善训练期间的梯度流。 尽管很有前途，但稀疏训练的能力尚未得到充分探索，DST 的潜在机制尚不清楚。 像这样的问题：为什么 Dynamic Sparse Training 可以提高稀疏训练的性能？ Dynamic Sparse Training 如何使稀疏神经网络模型能够匹配 - 甚至优于 - 密集模型？ 必须回答



## In-Time Over-Parameterization

在本节中，我们详细描述了实时过参数化，这是我们提出的一种概念，可以作为训练深度神经网络而无需昂贵的过参数化的替代方法。 我们将 In-Time OverParameterization 称为密集过度参数化的变体，这可以通过鼓励在整个训练时间内进行连续参数探索来实现。 需要注意的是，不同于稠密模型的过参数化指的是参数空间的空间维度，实时过参数化指的是在时空流形中探索的整体维度

### In-Time Over-Parameterization Hypothesis

基于实时过参数化，我们提出以下假设来理解动态稀疏训练：

假设。 **动态稀疏训练的好处在于它能够在搜索最佳稀疏神经网络连接时跨时间考虑所有可能的参数**。 具体来说，这个假设可以分为三个主要支柱来解释 DST 的性能：

1. 动态稀疏训练可以显着提高稀疏训练的性能，这主要归功于跨训练时间的参数探索。
2. 动态稀疏训练的性能与整个训练过程中可靠探索参数的总数高度相关。 可靠探索的参数是指那些新探索的（新生长的）权重，这些权重已经更新了足够长的时间以超过修剪阈值。
3. 只要有足够的可靠探索的参数，通过 Dynamic Sparse Training 训练的稀疏神经网络模型可以在很大程度上匹配甚至优于它们的密集模型，即使在极高的稀疏度水平下也是如此。

为了方便起见，我们将我们的假设命名为及时超参数化假设。

### Hypothesis Evaluation

在本节中，我们将研究实时过度参数化假设，并研究实时过度参数化对 DST 性能的影响。 我们选择稀疏进化训练 (SET) 作为我们的 DST 方法，因为 SET 以随机方式激活新权重，自然会考虑所有可能的参数进行探索。 它还有助于避免基于梯度的方法引入的密集过度参数化偏差，例如 Rigged Lottery (RigL)（Evci 等人，2020a）和 Sparse Networks from Scratch (SNFS)（Dettmers & Zettlemoyer，2019）， 因为后者在向后传递中利用密集梯度来探索新的权重。 为了验证提出的假设，我们进行了一组带有图像分类的逐步时尚实验。 我们在 CIFAR-10 上研究多层感知器 (MLP)，在 CIFAR-10 上研究 VGG-16，在 CIFAR-100 上研究 ResNet-34，在 ImageNet 上研究 ResNet-50。 我们使用 PyTorch 作为我们的库。 所有结果均取自三个不同运行的平均值，并用平均值和标准偏差报告。 实验细节见附录 A。

#### TYPICAL TRAINING TIME

我们对实时超参数化假设的第一个评估是看看在典型训练时间（200 或 250 个时期）内训练期间达到不同的超参数化率 Rs 时会发生什么。 控制 Rs 的一种直接方法是改变 ΔT，这是一个确定稀疏连接更新间隔（两次稀疏连接更新之间的迭代次数）的超参数。 我们使用各种 ΔT 训练 MLP、VGG-16 和 ResNet-34，并报告测试精度。

预期成绩。 逐渐减小 ΔT 将探索更多参数，从而导致越来越高的测试精度。 然而，当 ΔT 小于可靠探索阈值 T0 时，测试精度将开始下降，因为新权重无法接收足够的更新以超过修剪阈值。

实验结果。 为了更好地概览，我们在图 3 的最左侧一列中绘制了在不同稀疏度下实现的性能。为了更好地理解 Rs 与测试精度之间的关系，我们在图 3 的其余列中分别报告了与 ΔT 相关的最终 Rs .

总的来说，所有线路中都存在类似的模式。 从静态稀疏训练开始，稀疏训练始终受益于随着 ΔT 减小而增加的 Rs。 然而，测试精度在达到峰值后开始迅速下降，尤其是在高稀疏度（黄线和蓝线）下。 例如，即使 MLP 和 ResNet-34 最终以极小的 ΔT 值（例如 10、30）达到 100% 的探索率，它们的性能也比静态稀疏训练差得多。 这种行为完全符合我们的假设。 虽然小的 ΔT 鼓励稀疏模型最大限度地探索跨越密集模型的搜索空间，但实时过参数化提供的好处受到不可靠参数探索的严重限制。 有趣的是，不可靠探索对较低稀疏度（绿线）的负面影响小于对高稀疏度（黄线）的负面影响。 我们认为这是微不足道的稀疏性（Frankle 等人，2020a），因为其余模型仍然过度参数化以适应数据。

#### EXTENDED TRAINING TIME

到目前为止，我们已经了解了典型训练时间的测试准确度和 Rs 之间的权衡。 减轻这种权衡的一种直接方法是在使用大 ΔT 的同时延长训练时间。 我们训练 MLP、VGG-16 和 ResNet-34 以延长训练时间并使用大的 ΔT。 根据图 3 所示的权衡，我们安全地将 ∆T 选择为 MLP 的 1500、VGG-16 的 2000 和 ResNet-34 的 1000。除了训练时间之外，学习率计划的锚点也是 按相同的因子缩放。

预期成绩。 在这种情况下，我们预计，除了延长训练时间带来的好处外，稀疏训练也会从增加的 Rs 中显着受益。

实验结果。 结果如图 4 所示。没有参数探索的静态稀疏训练始终达到最低精度。 然而，不同稀疏度的所有模型都从延长的训练时间和增加的 Rs 中受益匪浅。 换句话说，及时可靠地探索参数空间不断提高稀疏训练的可表达性。 重要的是，在匹配密集基线（黑线）的性能后，稀疏训练的性能继续提高，比密集基线有显着改善。 此外，与稀疏度较高的模型相比，稀疏度较低的模型需要更少的时间来匹配其全精度平台； 原因似乎是稀疏度较低的模型可以在相同的训练时间内探索更多的参数。

为了表明性能提升不仅是由较长的训练时间引起的，我们通过在典型训练时间后立即停止参数探索来进行受控实验（典型训练时间后稀疏连接保持固定），如橙色虚线所示 线。 正如我们所看到的，即使有所改进，精度也远低于 In-Time OverParameterization 所达到的精度。

我们还将训练时间延长的密集模型的性能报告为黑色虚线。 训练具有延长时间的密集模型会导致劣质（MLP 和 VGG-16）或相等的解决方案（ResNet-34）。 与通常在模型过度训练足够长的时间后发生过度拟合的密集过度参数化不同，动态稀疏训练的测试精度随着 Rs 的增加而不断增加，直到达到完全实时过度参数化的平稳状态。 这一观察突出了及时过度参数化的优势，以防止过度拟合密集的过度参数化。



## Effect of Hyperparameter Choices

### Effect of Weight Growth methods on ITOP

我们接下来研究基于梯度的权重增长（用于 RigL 和 SNFS）和基于随机的权重增长（用于 SET）对实时超参数化的影响。 由于基于梯度的方法可以在反向传递中使用密集的过参数化（偶尔使用密集梯度来激活新的权重），我们假设它们可以在没有高 Rs 的情况下达到收敛精度。 我们在图 5 中对典型训练和扩展训练的 RigL 和 SET 进行了比较。我们在模型大小相对较小的 MLP 上研究它们，以便我们可以轻松实现完全实时过参数化并更好地理解 这两种方法中。

典型的培训时间。 很明显，RigL 也深受不可靠探索的影响。 随着ΔT的减小，RigL的测试精度呈现上升、下降、再上升的趋势。 与基于随机的增长相比，RigL 从可靠的参数探索中获得更大的收益，同时从不可靠的探索中获得更大的损失。 这些差异可能是由于 RigL 增加了具有高梯度幅度的新权重，这会导致在忠实探索时更快地减少损失，但也需要更高的 ΔT 来保证忠实探索，因为具有大梯度的权重可能 最终以高幅度结束，导致大的修剪阈值。

延长培训时间。 对于 RigL，我们选择 ΔT = 4000 以确保可靠的探索（具有较小 ΔT = 1500 的 RigL 的性能更差，如附录 D 所示）。 我们可以看到 RigL 也显着受益于增加的 Rs。 令人惊讶的是，尽管 RigL 在有限的训练时间下获得了比 SET 更高的精度，但在足够的训练时间下它最终的精度低于 SET。 从Rs的角度，我们可以看到RigL的Rs远小于SET，说明梯度权值的增长驱动稀疏连通性变成了一些相似的结构，进而限制了它的可表达性。 相反，随机增长自然会考虑整个搜索空间来探索参数，并且有更大的可能性找到更好的局部最优。 Liu 等人也报道了稀疏递归神经网络 (RNN) 的类似结果。 (2021)。 然而，类似的结果并没有与大型数据集上的大型架构共享。 例如，RigL 在 ImageNet 上使用 ResNet-50 实现了比 SET 更好的性能。 这个结果是合理的，因为密集梯度帮助 RigL 在每次稀疏连接更新时轻松找到最有希望的权重。 相比之下，SET（随机权重增长）需要更长的时间（高 Rs）才能在大规模架构中发现这些有前途的权重，尤其是在高稀疏度的情况下。

### Effect of Batch Size on ITOP

直观地说，我们的假设揭示了在有限的训练时间内改进现有 DST 方法的方法。 在典型训练时间内可靠地探索更多参数的一种直接方法是使用小批量进行训练。 使用较小的批量大小同样意味着有更多的更新，因此会导致更高的 Rs。 我们在图 6 中简单地证明了这一猜想对 SET 的有效性，其中 ∆T = 1500（RigL 参见附录 E）。 对于大批量，参数探索不足以实现高 InTime 过参数化率 Rs，因此测试精度远低于密集模型。 正如我们预期的那样，批量大小的减少会持续增加 Rs 以及测试精度，直到批量大小小于 16。但是，随着批量大小的减小，密集模型的性能会显着降低。 更有趣的是，当批大小小于 16 时，稀疏模型的性能发生翻转，最稀疏的模型开始达到最高准确度。 性能下降可能是由于 SGD 的“噪声尺度”增加造成的，其中极小的批量会导致较大的噪声尺度和较大的精度下降（Smith 等人（2017 年））。

### Effect of Pruning Rate on ITOP

参数探索的初始修剪率（表示为 P）也会影响训练期间访问的参数总数。 相对较大的剪枝率鼓励大范围的探索，从而获得更高的准确性，而太大的剪枝率会损害模型容量，因为它会剪掉太多参数。 我们用 CIFAR-10 上的 ResNet-18 证实了这一点，这些初始修剪率 P ∈ [0.1, 0.3, 0.5, 0.7, 0.9] 如表 1 所示。我们预期的相似模式在所有更新间隔中共享。

### Boosting the Performance of DST

基于上述见解，我们展示了 ResNet-50 在 ImageNet 上最先进的稀疏训练性能。 更准确地说，我们选择 4000 的更新间隔 ΔT，64 的批量大小和 0.5 的初始修剪率，以便我们可以在典型的训练时间内实现高 Rs。 我们将改进后的方法简称为 RigL-ITOP。 实施细节请参阅附录 B。 表 2 显示，在没有任何先进技术的情况下，我们的方法比基于过度参数化的方法（GMP 和彩票倒带 (LTR)（Frankle 等人，2020a））提高了 RigL 的准确性。 更重要的是，我们的方法只需要 2 倍的训练时间即可匹配稀疏度为 80% 的密集 ResNet-50 的性能，远低于 RigL（5 倍的训练时间）（Evci 等人，2020a）。

鼓励参数探索的另一个技巧不是使用小批量，而是在增加新权重时首先从非激活权重中采样。 我们在附录 F 中展示了这个想法的有效性。



## The Versatility of ITOP

虽然我们主要关注从 ITOP 的角度理解 DST，但 ITOP 可以潜在地推广到其他稀疏性诱导类别。 在这里，我们通过将 ITOP 应用于两种最近流行的方法 LTH 和 PI 来展示其多功能性。 我们选择 SNIP（Lee 等人，2019 年）作为 PI 方法，因为它在不同的初始化修剪方法中始终表现良好，如 Frankle 等人所示。 (2020b)。 与 ITOP 相比，LTH 和 SNIP 是两种基于过参数化的方法，旨在获得更好的初始子网络，但无需参考训练期间产生的任何信息。 我们为 SNIP 和 LTH 选择基于幅度的权重修剪和基于随机的权重增长来实现 ITOP，并将相应的方法命名为 SNIP-SET-ITOP 和 LTH-SET-ITOP。 为了公平比较不同的修剪标准，我们对 SNIP 和 LTH 使用全局和一次性修剪。 我们训练所有模型 200 个时期，并在图 7 中报告了最佳测试精度。有关实验详细信息，请参见附录 C。

凭借高实时超参数化率，SET-ITOP 始终大大优于基于超参数化的方法以及密集训练。 例如，SET-ITOP 可以轻松匹配最多 5% 参数的相应密集模型的性能。 更重要的是，SET-ITOP 在 LTH 和 SNIP 的极端稀疏度 (98%) 下具有优势性能，表明实时过参数化具有解决极端稀疏神经网络的可表达性差问题的潜力。

更有趣的是，In-Time OverParameterization 也为 SNIP 和 LTH 带来了巨大的好处。 虽然 LTH 和 SNIP 不及 SET-ITOP，但 SNIPSET-ITOP 和 LTH-SET-ITOP 可以与 MLP 和 ResNet34 相媲美甚至超过 SET-ITOP 的性能。 这一观察证实 ITOP 是一个基础概念，可以潜在地改进任何现有的稀疏训练方法。



## Generalization Improvement of ITOP

我们进一步观察了 In-Time OverParameterization 提高泛化能力的能力。 图 8 显示了 CIFAR-10 上实时过参数化（SET-ITOP 和 RigL-ITOP）的泛化误差（训练精度和测试精度之间的差异）和 MLP 的密集过参数化。 很明显，具有实时超参数化属性的模型比相应的密集模型具有更好的泛化能力。 随着模型变得更密集，泛化误差逐渐增加。 连同图 7 中的结果，我们可以看到稀疏度的降低导致更好的分类性能但更差的泛化。



## Conclusion and Future Work

在本文中，我们提出了实时过参数化，一种时空流形中密集过参数化的变体，作为训练深度神经网络的另一种方法，没有令人望而却步的密集过参数化依赖。 我们展示了实时过参数化的能力 (1) 提高稀疏训练的可表达性，(2) 加速训练和推理，(3) 理解 DST 的底层机制，(4) 防止过度拟合和提高泛化能力 . 此外，我们根据经验发现，通过充分可靠的参数探索，随机初始化的稀疏模型始终比那些专门初始化的静态稀疏模型具有更好的性能。 我们的论文表明，分配有限的资源来探索更多的稀疏连接空间比分配所有资源来寻找良好的稀疏初始化更有效和高效。

我们的论文发现了参数探索对于稀疏训练的重要性。 尽管我们调整了 RigL 的超参数并达到了最先进的稀疏训练性能，但小批量的使用减慢了现代架构的训练速度。 在典型的训练时间内追求大批量的高实时超参数化率是很有趣的。 此外，我们认为 ITOP 有潜力帮助人们解释网络的决策（Wong 等人，2021 年），提高分布和不确定性性能的稳健性（Zhang 等人，2021 年），检测非虚假信息 相关性 (Sagawa et al., 2020) 等。