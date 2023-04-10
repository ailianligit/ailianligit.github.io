# [MLSys 2019] Towards Federated Learning at Scale: System Design

## Introduction

- A basic design decision for a Federated Learning infrastructure is **whether to focus on asynchronous or synchronous**
  **training algorithms**.
- support for **synchronous rounds**, while mitigating potential **synchronization overhead**
- Practical Issues:
  - device availability that correlates with the local data distribution in complex ways (e.g., time zone dependency)
  - unreliable device connectivity and interrupted execution
  - orchestration of lock-step execution across devices with varying availability
  - limited device storage and compute resources
- the first **production-level** Federated Learning implementation, focusing primarily on the **Federated Averaging** algorithm running on **mobile phones**.



## Protocol

![image-20221027174309621](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498647.png)



## Device

![image-20221027174340432](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498662.png)



## Server

![image-20221027174437425](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498667.png)



## Tools and Workflow

![image-20221027174547920](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498674.png)

