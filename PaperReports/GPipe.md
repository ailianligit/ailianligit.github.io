# [GPipe@NeurIPS 2019] Efficient Training of Giant Neural Networks using Pipeline Parallelism

## Abstract

- increasing model capacity beyond the memory limit of a single accelerator
- efficient and task-independent model parallelism
- pipeline parallelism library
  - allows scaling any network that can be expressed as **a sequence of layers**
- pipelining different sub-sequences of layers on separate accelerators
  - flexibility of scaling a variety of different networks to gigantic sizes efficiently
- batch-splitting pipelining algorithm
  - almost linear speedup when a model is partitioned across multiple accelerators  



## Introduction

- Hardware constraints: **memory limitations** and **communication bandwidths** on accelerators (GPU or TPU)
- tradeoff between scaling capacity, flexibility (or specificity to particular tasks and architectures) and training efficiency
- scaling arbitrary deep neural network architectures beyond the memory limitations of a single accelerator
- partitioning the model across different accelerators and supporting re-materialization on every accelerator
- **batch splitting**
- gradient updates using GPipe are **consistent** regardless of the number of partitions



![image-20221031002933938](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/images/images/202212/20221206_1670320508.png)



## Conclusion

- Efficiency, flexibility & reliability