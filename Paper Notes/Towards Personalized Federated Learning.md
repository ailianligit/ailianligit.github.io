# [Survey] Towards Personalized Federated Learning

## Abstract

- fundamental challenges of FL: heterogeneous data
  - statistical heterogeneity under the FL setting  



## Introduction

![image-20221030205651980](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221206_1670321552.png)



### Categorization of Federated Learning

- horizontal FL (HFL)
- vertical FL (VFL)
- federated transfer learning (FTL)



### Motivations for Personalized Federated Learning

- #### Poor Convergence on Heterogeneous Data

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221206_1670321564.png" alt="image-20221030230605485" style="zoom:50%;" />

- #### Lack of Solution Personalization



## Strategy I: Global Model Personalization

![image-20221030230916781](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221206_1670321587.png)

### Data-Based Approaches

- client drift problem: reduce the statistical heterogeneity of client data distributions
- need to modify the local data distributions: result in the loss of valuable information associated with
  **the inherent diversity of client behaviors**

#### Data Augmentation

#### Client Selection



### Model-Based Approaches

- learn a strong global FL model for future personalization on each individual client
- improve the adaptation performance of the local model

#### Regularized Local Loss

- ##### Between Global and Local Models

- ##### Between Historical Local Model Snapshots

#### Meta-learning

#### Transfer learning

![image-20221030232323201](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221206_1670321600.png)



## Strategy II: Learning Personalized Models

![image-20221030232547263](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221206_1670321609.png)

### Architecture-Based Approaches

#### Parameter Decoupling: personalization layers

#### Knowledge Distillation: personalized model architectures



### Similarity-Based Approaches: modeling client relationships

#### Multitask Learning: pair-wise

#### Model Interpolation: pair-wise

#### Clustering: group-level

![image-20221030232726107](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221206_1670321621.png)



## PFL Benchmark and Evaluation Metrics



## Promising Future Research Directions

### Opportunities for PFL Architectural Design

- Client Data Heterogeneity Analytics: The problem of FL client **data heterogeneity analysis** in a **privacy-preserving** manner
- Aggregation Procedure: Specialized aggregation procedures for PFL
- PFL Architecture Search: NAS (**parameter decoupling** and **KD-based** PFL methods)
- Spatial Adaptability: handle variations across client datasets
  - catastrophic forgetting: incorporate continual learning into FL
  - formalize the trade-offs between overhead (communication) and performance
- Temporal Adaptability: learn from nonstationary data
  - leverage existing **drift detection** and adaptation algorithms to improve learning on dynamic real-world data



### Opportunities for PFL Benchmarking



### Opportunities for Trustworthy PFL

- Open Collaboration: **Incentive mechanism** design: Game theory, pricing, and auction mechanisms
- Fairness
- Explainability: tradeoff between **explainability and privacy**: incorporate explainability into the global FL model but not the personalization component of the FL model
- Robustness: attacks and defenses

