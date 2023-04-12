# [Survey] A Survey on Security and Privacy of Federated Learning

## Abstract

- Security threats: communication bottlenecks, poisoning, backdoor attacks
- privacy threat: inference-based attack



## Introduction

- hacking & curious attacks: model parameter sharing & an increased number of training iterations and communications



## Federated learning landscape

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498740.png" alt="image-20221101164942548" style="zoom:50%;" />



## FL categorization of techniques/approaches

<img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498753.png" alt="image-20221101165032156" style="zoom:50%;" /><img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498777.png" alt="image-20221101165431518" style="zoom:50%;" /><img src="https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498789.png" alt="image-20221101165444868" style="zoom:50%;" />



## Security in federated learning

- Confidentiality, Integrity, and Availability



### Source of vulnerabilities in the FL ecosystem

- Communication Protocol
- Client Data Manipulations
- Compromised Central Server
- Weaker Aggregation Algorithm
- Implementer’s of FL Environment



### Security threats/attacks in FL domain

- Poisoning
  - Data Poisoning
  - Model Poisoning
  - Data Modification
- Inference
- Backdoor attacks
- GANs
- System disruption IT downtime
- Malicious server
- Communication bottlenecks
- Free-riding attacks
- Unavailability
- Eavesdropping
- Interplay with data protection laws

![image-20221101191526611](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498804.png)



### Defensive techniques for security vulnerabilities of FL

- proactive: **guess** the threats and risks associated with and employ a **cost-effective** defense technique
- reactive: **later work done** when an attack is identified and as part of the mitigation process, where a defense technique is deployed as **patch-up**
  in the production environment
- blockchain
- Sniper
- **Knowledge distillation**
- Anomaly detection
- Moving target defense
- **Federated MultiTask Learning**
- Trusted Execution Environment (TEE)
- Data Sanitization
- Foolsgold
- **Pruning**: address backdoor attacks and communication bottlenecks

![image-20221101193849823](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498811.png)



## Privacy in federated learning

### Privacy threats/attacks in FL domain

- Membership inference attacks
- Unintentional data leakage & reconstruction through inference
- GANs-based inference attacks



### Techniques to mitigate identified threats and enhance the general privacy-preserving feature of FL

- Secure multi-party computation
- Differential privacy
- VerifyNet
- Adversarial training

![image-20221101194213220](https://raw.githubusercontent.com/ailianligit/ailianligit.github.io/main/images/202212/20221208_1670498820.png)



## Future directions in FL security and privacy

- Zero-day adversarial attacks and their supporting techniques
- Trusted traceability
- Well-defined process with APIs
- Optimize trade-off between privacy protection enhancement and cost
- Build FL privacy protection enhanced frameworks in practice
- Client selection and training plan in FL
- Optimization techniques for different ML algorithms
- Vision on training strategies and parameters
- Ease in migrating and productionising
