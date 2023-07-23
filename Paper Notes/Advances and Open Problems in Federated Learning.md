# [Survey] Advances and Open Problems in Federated Learning

## 1 Introduction

- An **unbalanced and non-IID** **data partitioning** across **a massive number of unreliable devices with limited communication bandwidth**

- cross-device: mobile and edge device
- cross-silo: involve only a small number of relatively reliable clients
- Federated learning is a machine learning setting where multiple entities (clients) collaborate in solving a machine learning problem, under the coordination of a central server or service provider. Each client’s raw data is stored locally and not exchanged or transferred; instead, focused updates intended for immediate aggregation are used to achieve the learning objective.



### 1.1 The Cross-Device Federated Learning Setting

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498603.png" alt="image-20221027222257247" style="zoom: 67%;" />

 

#### 1.1.1 The Lifecycle of a Model in Federated Learning

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498611.png" alt="image-20221027223632459" style="zoom:50%;" />

- Problem identification - Client instrumentation - Simulation prototyping (optional) - Federated model training - (Federated) model evaluation - Depolyment



#### 1.1.2 A Typical Federated Training Process

- Client selection - Broadcast - Client computation - Aggregation - Model update
- synchronous vs asynchronous



### 1.2 Federated Learning Research

- Precisely describe the details of the particular FL setting of interest
- Explain which aspects of the real-world setting the simulation is designed to capture (and which it is not)
- **Privacy** and **communication efficiency**: **where computation happens** and **what is communicated**



<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498628.png" alt="image-20221031221059147" style="zoom:50%;" />