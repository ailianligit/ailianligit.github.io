# [Sub-FedAvg@ICDCSW'21] Personalized Federated Learning by Structured and Unstructured Pruning under Data Heterogeneity

## Abstract

The traditional approach in FL tries to learn a single global model collaboratively with the help of many clients under the orchestration of a central server. However, learning a single global model might not work well for all clients participating in the FL under data heterogeneity. Therefore, the personalization of the global model becomes crucial in handling the challenges that arise with statistical heterogeneity and the non-IID distribution of data. Unlike prior works, in this work we propose a new approach for obtaining a personalized model from a client-level objective. This further motivates all clients to participate in federation even under statistical heterogeneity in order to improve their performance, instead of merely being a source of data and model training for the central server. To realize this personalization, we leverage finding a small subnetwork for each client by applying hybrid pruning (combination of structured and unstructured pruning), and unstructured pruning. Through a range of experiments on different benchmarks, we observed that the clients with similar data (labels) share similar personal parameters. By finding a subnetwork for each client rather than taking the average over all parameters of all clients for the entire federation as in traditional FL, we efficiently calculate the averaging on the remaining parameters of each subnetwork of each client. We call this novel parameter averaging as Sub-FedAvg. Furthermore, in our proposed approach, the clients are not required to have knowledge of any underlying data distributions or label similarities among the rest of clients. The non-IID nature of each client’s local data provides distinguishing subnetworks without sharing any data. We evaluate our method on federated image classification with real world datasets. Our method outperforms existing state-of-the-art.



- 通过对不同基准的一系列实验，我们观察到具有相似数据（标签）的客户端共享相似的个人参数。
- Sub-FedAvg：
  - 通过应用混合修剪（结构化和非结构化修剪的组合）和非结构化修剪，为每个客户端寻找一个小的子网。将structued剪枝应用于卷积层，将unstructed剪枝应用于全连接层。
  - 通过为每个客户端找到一个子网，而不是像传统 FL 那样对整个联邦的所有客户端的所有参数取平均值，有效地计算了每个客户端的每个子网的剩余参数的平均值。
  - 服务器收到客户端的模型后只在模型参数交集中进行聚合。
- 客户端不需要了解其他客户端之间的任何基础数据分布或标签相似性。
- 每个客户端本地数据的非 IID 性质提供了不同的子网，而无需共享任何数据。 

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202302/20230219_1676791317.gif" alt="img" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202302/20230219_1676791347.gif" alt="img"  />