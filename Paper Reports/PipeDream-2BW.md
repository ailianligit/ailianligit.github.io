# [PipeDream-2BW@ICML 2021] Memory-Efficient Pipeline-Parallel DNN Training

## Abstract

- **pipelining** and **weight gradient coalescing** strategy, combined with the **double buffering of weights**
  - ensure **high throughput**, **low memory footprint**, and **weight update semantics similar to data parallelism**
- **automatically partitions** the model over the available hardware resources, while respecting hardware constraints such as **memory capacities** of accelerators and **interconnect topologies**



## Introduction

- trade off **memory footprint** and **throughput**
  - GPipe: limit overall throughput
  - PipeDream: increases the memory footprint
- Challenge: Memory Capacity Constraints, Heterogeneous Network Interconnects, Large Search Space for Operator Placement
- double-buffered weight updates (2BW): 
  - reduces the **memory footprint** of training while avoiding **pipeline flushes**
- Planner:
  - partitions DNN operators over the **available** workers while taking into account the **memory capacities** of the accelerator devices
  - determines the size of each model partition, batch size, and whether to use memorysaving optimizations like activation recomputation



![image-20221031015258308](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20221031015258308.png)