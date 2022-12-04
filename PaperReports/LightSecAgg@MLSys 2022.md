# [LightSecAgg@MLSys 2022] LightSecAgg: a Lightweight and Versatile Design for Secure Aggregation in Federated Learning

## Abstract

- **one-shot** **aggregate-mask reconstruction** of the active users via **mask encoding/decoding**
- same **privacy** and dropout-**resiliency** guarantees
- reducing the **overhead** for resiliency against dropped users
- a modular system design and optimized on-device parallelization for **scalable** implementation
  - enabling **computational overlapping** between model training and on-device encoding
  - improving the speed of concurrent receiving and sending of chunked masks