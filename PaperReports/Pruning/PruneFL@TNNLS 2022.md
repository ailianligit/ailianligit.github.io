# [PruneFL@TNNLS 2022] Model Pruning Enables Efficient Federated Learning on Edge Devices

## Abstract

- challenge: much more limited **computation** and **communication** resources of client devices
- PruneFL—a novel FL approach with **adaptive** and distributed parameter pruning, which adapts the **model size** during FL to reduce both communication and computation overhead and minimize the overall training time, while maintaining a similar accuracy as the original model
- PruneFL includes **initial pruning at a selected client** and **further pruning** as part of the FL process
- The model size is adapted during this process, which includes **maximizing the approximate empirical risk reduction divided by the time of one FL round**
- experiments
  - reduce the training time compared to conventional FL and various other pruning-based methods 
  - the pruned model with automatically determined size converges to an accuracy that is very similar to the original model, and it is also a lottery ticket of the original model



## Introduction

- computation power, communication bandwidth, memory and storage size, and so on