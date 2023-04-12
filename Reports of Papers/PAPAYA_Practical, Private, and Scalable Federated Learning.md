# [PAPAYA@MLSys'22] PAPAYA: Practical, Private, and Scalable Federated Learning

## Abstract

- Asynchronous FL: faster & less communication overhead
- Papaya: the first production FL system to support asynchronous and synchronous training at scale.  



## Introduction

- Scalability: a measure of FL systems' capacity to effectively utilize an increasing number of processors
- SyncFL
  - many sources of **heterogeneity**
  - increasing cohort size does not reduce wall-clock training time proportionally

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498702.png" alt="image-20221114203605278" style="zoom:50%;" />

- AsyncFL: Papaya
  - **secure** aggregation protocol
  - less **staleness**
  - compute many more **server updates** than SyncFL in a fixed amount of time, leading to much **better** **scaling** than SyncFL
  - the number of server updates per unit time increases nearly **linearly** with concurrency



## Understanding the Landscape of Federated Learning at-Scale

- System and data heterogeneity
  - stragglers - over-selection - sampling bias and unfair to stragglers

- Scalability
  - the training time to convergence
  - the communication overhead  



## Proposed Design

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498711.png" alt="image-20221114203518339" style="zoom:50%;" />

## System Components

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498717.png" alt="image-20221114203956240" style="zoom:50%;" />



## Secure Aggregation



## System Design

### Client Independence

### High Client Utilization

### Fast Model Aggregation



## Evaluation