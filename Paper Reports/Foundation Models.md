# [Report] On the Opportunities and Risks of Foundation Models

## Abstract

AI is undergoing a paradigm shift with the rise of models (e.g., BERT, DALL-E, GPT-3) that are **trained on broad data at scale** and are **adaptable to a wide range of downstream tasks**. We call these models foundation models to underscore their critically central yet incomplete character. This report provides a thorough account of the opportunities and risks of foundation models, ranging from their capabilities (e.g., language, vision, robotics, reasoning, human interaction) and technical principles (e.g., model architectures, training procedures, data, systems, security , evaluation, theory) to their applications (e.g., law, healthcare, education) and societal impact (e.g., inequity, misuse, economic and environmental impact, legal and ethical considerations). Though foundation models are based on **standard deep learning and transfer learning**, their scale results in new emergent capabilities,and their effectiveness across so many tasks incentivizes homogenization. **Homogenization** provides powerful leverage but demands caution, as the defects of the foundation model are inherited by all the adapted models downstream. Despite the impending widespread deployment of foundation models, we currently lack a clear understanding of how they work, when they fail, and what they are even capable of due to their emergent properties. To tackle these questions, we believe much of the critical research on foundation models will require deep interdisciplinary collaboration commensurate with their fundamentally sociotechnical nature.



## Introduction

- A *foundation model* is any model that is **trained on broad data** (generally using self-supervision at scale) that can be adapted (e.g., fine-tuned) to **a wide range of downstream tasks**
- DNN & self-supervised learning



## Capabilities

- multi-purpose nature, strong generative and multimodal capabilities

### Language

- *linguistic variation*
- child *language acquisition*

### Vision

- models pretrained on large annotated datasets can transfer to numerous downstream settings

### Robotics

- fundamental challenges due to being anchored to the physical world
- how plentiful data (*e.g.,* generic videos of humans, amongst others) that is not specific to particular environments and across modalities (*e.g.,* language, vision)
- allow for easier task specification and learning
- ushering in new applications (e.g., better robotic assistance for household tasks) 
- heightening the importance of robustness and safety (e.g., formal safety evaluation)

### Reasoning and search

- control the combinatorial explosion inherent to search

### Interaction

- sample efficiency in adaptation
- raise the ceiling for *novel user interaction*
- developers can provide applications that better fit the *user's needs and values*, while introducing far more dynamic forms of interaction and opportunities for *feedback*.

### Philosophy of understanding



## Applications

###### data, privacy, interpretability, fairness and ethics

- ### Healthcare and biomedicine

- ### Law

- ### Education



## Technology

### Modeling

- *expressivity*: capture and assimilate real-world information
- *scalability*: adeptly handle large quantities of high-dimensional data
- *multimodallity*: consume, process and potentially produce content from different sources and domains
- *memory* capacity: effectively store and retrieve the acquired knowledge
- *compositionality*: foster successful generalization to novel settings and environments



### Training

- training objectives:
  - *principled selection*: derived from systematic evidence and evaluation
  - *domain-generality*: provide rich, scalable, and unified training signal across data sources and modalities



### Adaptation

- accuracy-efficiency tradeoffs: lightweight fine-tuning alternatives and prompting-based methods
- Future: alleviate deficiencies of stand-alone foundation models (*e.g.,* *temporal adaptation* to reflect changes over time in the world) or introduce *constraints* (*e.g.,* GDPR compliance relating to the *right to be forgotten*)
- systematically evaluate adaptation methods while controlling for resources (*e.g.,* runtime, memory) and access requirements involved in adaptation



### Evaluation

- evaluating foundation models *directly* to measure their *inherent capabilities* and inform how foundation models are trained
- evaluating task-specific models by *controlling for adaptation resources and access*
- broader *evaluation design* to provide richer context beyond measures of accuracy (*e.g.,* robustness, fairness, efficiency, environmental impact)



### Systems

- from carefully tuned **parallelism strategies** to new architectures such as **retrieval-based** and **mixture-of-expert** models



### Data

- selection, curation, documentation, access, visualization and inspection, quality assessment, and legal regulation



### Security and privacy

- security vulnerabilities (*e.g.,* adversarial triggers to generate undesirable outputs)
  - **a strong abstraction layer** to build upon for reliable ML applications
- privacy risks (*e.g.,* memorization of training data)
  - **knowledge transfer** from public data: foundation models may enable more sample efficient adaptation to sensitive data distributions



### Robustness to distribution shifts

- adapting **a foundation model** trained on **a broad range of unlabeled data** improves the robustness of adapted models across **a wide variety of shifts**



### AI safety and alignment

- aligning foundation models: they are not deployed with misspecified goals or values
- forecasting the emergent behaviors of foundation models (e.g., the ability to deceive or plan strategically)



### Theory

- the **training** phase and the **adaptation** phase: correspond to (potentially) completely different tasks and data distributions



### Interpretability

- *one model-many models*: determine the extent to which the *one model* (the foundation model) and its *many models* (its adapted derivatives) share decision-making building blocks
- explainability: e.g., the validity of post hoc explanations generated by models
- mechanisms that drive model behavior (which may clarify the extent to which understanding foundation models can extend to understanding their adapted derivatives)



## Society

- ### Inequity and fairness

- ### Misuse

- ### Environment

- ### Legality

- ### Economics

- ### Ethics of scale



## My Opinion

- *expressivity*: capture and assimilate real-world information
- *multimodallity*: consume, process and potentially produce content from different sources and domains
- *scalability*: adeptly handle large quantities of high-dimensional data
- *compositionality*: foster successful generalization to novel settings and environments
  - adaption, multi-task learning and transfer learning
- *memory* capacity: effectively store and retrieve the acquired knowledge
  - model at scale
- pre-training
- **a strong abstraction layer** to build upon for reliable ML applications
- the **training** phase and the **adaptation** phase
- 旧调重弹 or 新的AI范式（搭建和部署模式）



## Related Topics

#### ***3 Applications - 3.1 Healthcare and biomedicine - 3.1.3 Challenges and future research in foundation models***

**Legal and ethical regulations.** Healthcare applications must observe legal and ethical regulations with guarantees, such as patient safety, privacy and fairness. For instance, regarding safety, predictions made by foundation models must be factually accurate with established medical knowledge, and must quantify uncertainty or choose to defer to an expert when uncertain [Challen et al. 2019; Mozannar and Sontag 2020]. For privacy, the use of patient health records must observe the privacy laws, such as HIPAA [Act 1996] in the case of the US. **Federated learning is one potential solution to keeping the raw, sensitive data private in the training of foundation models** [Chamikara et al. 2021]. For fairness, researchers will need to be mindful of common pitfalls or otherwise risk exacerbating existing social inequalities [Chen et al. 2019; Wiens et al. 2019; Chen et al. 2020b]. They must 58 Center for Research on Foundation Models (CRFM) ensure that the training and evaluation data for foundation models is sufficiently representative of different sexes, races, ethnicities and socioeconomic backgrounds; an area where medical datasets and clinical trials have had a long history of bias [Martinez-Martin et al. 2020; Kaushal et al. 2020]. Research is also needed to debias and regularize models to ensure fairness when representative data is scarce [Zhao et al. 2020a]. Foundation model developers also need to consult with ethics and law researchers, and observe regulations in the specific circumstances (e.g., country, region) where they are deployed. We also refer readers to §4.7: security, §4.8: robustness, §5.1: fairness, §5.4: legality for details on privacy, robustness, fairness and legality.  

#### ***4 Technology - 4.1 Modeling - 4.1.2 Scalability***

**Hardware Compatibility.** Moving beyond aspects of robustness and optimization, foundation models should also be practically efficient (§4.5: systems), and take advantage of contemporary and future hardware [Hooker 2020]. One example of that is parallelizablity, an important property that characterizes the computation supported by GPUs. Indeed, much of the transformers’ great success over the previously dominating recurrent approach was driven by their higher degree of parallelism. Looking forward, given the fast-pace progress of systems development, we should further ensure that models are designed to co-adapt to future hardware advances. Consequently, foundation models should ideally be amenable to schemes such as **distributed training**, which is gaining popularity, as is the case for e.g., **Mixture-of-Experts**, and possibly leverage properties such as sparsity of the computation or representation, as is the case for the Longformer [Beltagy et al. 2020], BigBird [Zaheer et al. 2020], and Sparse Transformer [Child et al. 2019] approaches, and which likely will become more central in future hardware and processors.

#### ***4 Technology - 4.1 Modeling - 4.1.5 Compositionality***

**Computation.** Models such as Module Networks [Andreas et al. 2016] and **Mixture-of-Experts** [Shazeer et al. 2017] go further along this direction, exhibiting not only structural modularity, but also compositional computation, supported by the specialization of sub-networks to different operations, in a manner that adapts and tailors the model behavior to the input at hand. While some methods rely on concatenation of hand-engineered modules [Andreas et al. 2016], alternative approaches enable the network specialization to naturally emerge through learning [Shazeer et al. 2017]. Other models, such as MAC [Hudson and Manning 2018] and Dynamic Memory Networks [Xiong et al. 2016] perform an explicit iterative computation, where a given task is decomposed into multiple reasoning steps, performed one by one, manifesting sequential progression from a set of initial facts to novel inferences and conclusions.

#### ***4 Technology -  4.5 Systems - 4.5.1 Improving performance through co-design***

While we will continue to realize performance gains from new hardware, growth in the resource requirements of large models far outstrips generational hardware improvements [Brown et al. 2020]. To facilitate the next major leap in model capacity and to democratize the advances in model quality, it will be increasingly critical to co-design training algorithms, models, software, and hardware, because many of the avenues to dramatically increase performance alter the semantics of the training computation. For example, executing operations in lower precision (such as fp16) can help increase throughput on modern hardware (e.g., the V100 and A100 GPUs have dedicated tensor core units for lower-precision matrix multiplication), but also affect the numerics of the optimization procedure [Micikevicius et al. 2017]. Similarly, exploiting weight sparsity can significantly improve training and inference times [Elsen et al. 2020; Gale et al. 2020] by only performing mathematical operations on the non-zeros in the model, but requires different training algorithms [Jayakumar et al. 2021; Evci et al. 2020; Dettmers and Zettlemoyer 2019]. Other examples of co-design include model architectures that map more efficiently to hardware [So et al. 2019; Child et al. 2019; Wang et al. 2020c; Lee-Thorp et al. 2021; Kitaev et al. 2020; Beltagy et al. 2020; Tay et al. 2020; Ren et al. 2021], efficient optimizers [Anil et al. 2020; Shazeer and Stern 2018], novel tokenization alternatives [Xue et al. 2021; Tay et al. 2021], specially architected hardware training platforms [Jouppi et al. 2017; Mudigere et al. 2021; Selene 2021], and **distributed parallelization strategies with relaxed weight update semantics** [Narayanan et al. 2019, 2021a]. 

#### ***4 Technology -  4.5 Systems - 4.5.3 Execution and programming models***

Building and maintaining a cluster of thousands of accelerators also requires tremendous effort. New training paradigms like Learning@Home [Ryabinin and Gusev 2020; Diskin et al. 2021] explore **leveraging volunteer compute over the internet to train foundation models collaboratively**. Such fundamentally new execution models can decrease the cost of training for any one entity, but require collaboration across a number of different areas like security (to ensure that a malicious volunteer cannot significantly alter the training process), **distributed systems (to deal with fault tolerance issues as volunteers drop)**, and crowdsourcing.

#### *4 Technology - 4.5 Systems - 4.5.4 Productionization of foundation models*

For applications with strict cost and latency constraints, **model compression techniques** like **distillation** [Hinton et al. 2015; Li et al. 2020d; Sanh et al. 2019], **quantization** [Polino et al. 2018; Gholami et al. 2021; Zhou et al. 2018], **pruning** [LeCun et al. 1990; Gordon et al. 2020; McCarley et al. 2019; Wang et al. 2019c; Sajjad et al. 2020], and **sparsity** [Gale et al. 2020; Elsen et al. 2020] could aid deployment by transforming larger models to obtain desired inference-time properties. **Parallelization techniques** like **tensor model parallelism** [Shoeybi et al. 2019], traditionally used for training, might also be useful to reduce inference latency, and also provide additional memory capacity across GPUs to fit the model’s parameters.