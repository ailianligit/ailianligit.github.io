# [FedGKT@NeurIPS 2020] Group Knowledge Transfer Federated Learning of Large CNNs at the Edge

## Abstract

- A variant of the alternating minimization approach.

- Advantages:

  - Reduced demand for edge computation and memory (SL)


  - Lower communication bandwidth for large CNNs (FL)


  - Reduces the communication bandwidth requirement (SL)


  - Asynchronous training (FL)




## Introduction

- **Training in resource-constrained edge devices**:

  - the computational demand of large CNNs

  - the meager computational power on edge devices

- **Federated Learning**:

  - **Reduce communication frequency** by local SGD and model averaging

  - Small CNNs

- **Split Learning**:

  - Partitions a large model and offloads some portion of the neural architecture to the cloud

  - Straggler problem: a single mini-batch iteration requires multiple rounds of communication between the server and edges

- FedGKT
  - Local SGD in FedAvg
  - Low compute demand at the edge in SL
  - Group knowledge transfer: reformulates FL as an alternating minimization (AM) approach, which optimizes two random variables (the edge model and the server model) by alternatively fixing one and optimizing another.

![image-20221026160028772](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221026160028772.png)



## Group Knowledge Transfer

![image-20221026160427801](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221026160427801.png)

![image-20221026160609497](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221026160609497.png)



## Experiments

## Discussion
