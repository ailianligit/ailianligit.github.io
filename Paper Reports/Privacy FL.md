# [Survey] A Survey on Security and Privacy of Federated Learning

## Abstract

- Security threats: communication bottlenecks, poisoning, backdoor attacks
- privacy threat: inference-based attack



## Introduction

- hacking & curious attacks: model parameter sharing & an increased number of training iterations and communications



## Federated learning landscape

<img src="C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101164942548.png" alt="image-20221101164942548" style="zoom:50%;" />



## FL categorization of techniques/approaches

<img src="C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101165032156.png" alt="image-20221101165032156" style="zoom:50%;" /><img src="C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101165431518.png" alt="image-20221101165431518" style="zoom:50%;" /><img src="C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101165444868.png" alt="image-20221101165444868" style="zoom:50%;" />



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

![image-20221101191526611](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101191526611.png)



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

![image-20221101193849823](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101193849823.png)



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

![image-20221101194213220](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101194213220.png)



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



# [Survey] Privacy and Robustness in Federated Learning: Attacks and Defenses

![image-20221101204345196](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101204345196.png)



## Privacy Attacks

- the gradients are derived from the participants’ private training data
- a learning model can be considered as a representation of the high-level statistics of the dataset
- target at **data** privacy

<img src="C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101204411118.png" alt="image-20221101204411118" style="zoom:50%;" />



## Defenses Against Privacy Attacks

<img src="C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101204453158.png" alt="image-20221101204453158" style="zoom:50%;" />

<img src="C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101205140960.png" alt="image-20221101205140960" style="zoom:67%;" />



## Poisoning Attacks

- compromise the **system** robustness
- untargeted poisoning attacks
  - data poisoning
  - model poisoning
- targeted poisoning attacks
- data poisoning attacks are generally less effective than model poisoning attacks   

<img src="C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101205159792.png" alt="image-20221101205159792" style="zoom:50%;" />



## Defenses Against Poisoning Attacks

![image-20221101205319092](C:\Users\wudic\AppData\Roaming\Typora\typora-user-images\image-20221101205319092.png)