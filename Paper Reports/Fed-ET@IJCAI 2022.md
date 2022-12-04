# [Fed-ET@IJCAI 2022] Heterogeneous Ensemble Knowledge Transfer for Training Large Models in Federated Learning

## Abstract

- Ensemble knowledge transfer
- Fed-ET uses a **weighted consensus distillation** scheme with **diversity regularization** that efficiently **extracts reliable consensus from the ensemble** while **improving generalization by exploiting the diversity** within the ensemble.
  - Weighted consensus distillation: clients with higher inference confidence contribute more to the consensus compared to less confident
    clients, extracts reliable consensus from the ensemble
  - Diversity regularization: clients that do not follow the consensus can still transfer useful representations to the server model, improving generalization by exploiting the diversity.



## Introduction

- Challenges in FL:
  - limited system resources
  - heterogeneous local datasets

- A naive aggregation of the clients’ models can hinder the **convergence** of the model due to **high data-heterogeneity** across the clients.
- Allowing different models to be deployed across clients depending on their system resources, all while training a larger model for the server.
- Clients return models not only trained on heterogeneous data, but also with different architecture.
- Fed-ET, the first **ensemble transfer** algorithm for FL using **unlabeled data** that enables training a large server model with smaller models at the clients, for any **classification** task.
- Consider the **data-heterogeneity** in FL by proposing a **weighted consensus distillation** approach with **diversity regularization**  



## Federated Ensemble Transfer: Fed-ET

- clients’ local training and representation transfer
- weighted consensus distillation with diversity regularization
- server representation transfer

![image-20221026221708534](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221026221708534.png)

![image-20221026221948950](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221026221948950.png)



## Experiments



## Conclusion

- deploying strategies of heterogeneous models to the clients

- extending Fed-ET to a general ensemble knowledge transfer framework