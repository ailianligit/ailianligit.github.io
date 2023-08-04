# Physical Storage Systems

## Overview of Physical Storage Media

### Classification of Physical Storage Media

- Can differentiate storage into: volatile storage & non-volatile storage
- Factors affecting choice of storage media include: **Speed** with which data can be accessed; **Cost** per unit of data; **Reliability**



### Storage Hierarchy

- **Primary storage**: Fastest media but volatile (cache, main memory)
- **Secondary storage**: next level in hierarchy, non-volatile, moderately fast access time, also called **on-line storage** (flash memory, magnetic disks)
- **Tertiary storage**: lowest level in hierarchy, non-volatile, slow access time, also called **off-line storage** and used for **archival storage** (magnetic tape, optical storage)

![image-20211020013945968](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079400.png)



## Storage Interfaces

- Disk interface standards families: SATA (Serial ATA), SAS (Serial Attached SCSI) & NVMe (Non-Volatile Memory Express) interface
- Disks usually connected directly to computer system:
  - In **Storage Area Networks (SAN)**, a large number of disks are connected by a **high-speed network** to a number of servers
  - In **Network Attached Storage (NAS)** networked storage provides a **file system interface using networked file system protocol**, instead of providing a disk system interface



## Magnetic Disks

![image-20211022093550927](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079397.png)

- A **sector** is the **smallest** unit of data that can be read or written
- To read/write a sector, disk arm swings to position head on right track, platter spins continually; data is read/written as sector passes under head
- **Disk controller**: interfaces between the **computer system** and the **disk drive hardware**
  - accepts high-level **commands** to read or write a sector
  - initiates **actions** such as moving the disk arm to the right track and actually reading or writing the data
  - Computes and attaches **checksums** to each sector to verify that data is read back correctly
  - Ensures successful writing by **reading back** sector after writing it
  - Performs **remapping of bad sectors**



### Performance Measures of Disks

- **Access time**: the time it takes from when a read or write request is issued to when data transfer begins
  - **Seek time**: time it takes to reposition the arm over the correct track
  - **Rotational latency**: time it takes for the sector to be accessed to appear under the head
- **Data-transfer rate**: the rate at which data can be retrieved from or stored to the disk
- **Disk block**: a logical unit for storage allocation and retrieval
- **Sequential access pattern**: Successive requests are for successive disk blocks; Disk seek required only for first block
- **Random access pattern**: Successive requests are for blocks that can be anywhere on disk; Each access requires a seek
- **I/O operations per second (IOPS)**: Number of random block reads that a disk can support per second
- **Mean time to failure (MTTF)**: the average time the disk is expected to run continuously without any failure



## Flash Memory

- **NAND** flash: used widely for storage, **cheaper** than NOR flash; requires **page-at-a-time** read; **Not much** difference between sequential and random read; Page can only be **written once**, must be erased to allow **rewrite**
- **Solid state disks**: Use standard **block-oriented** disk interfaces, but store data on **multiple flash** storage devices internally
- Erase happens in units of **erase block**
- **Remapping** of logical page addresses to physical page addresses avoids waiting for erase
- **Flash translation table** tracks mapping, also stored in a **label field of flash page**, remapping carried out by **flash translation layer**
- **wear leveling**: After 100,000 to 1,000,000 **erases**, erase block becomes unreliable and cannot be used

![image-20211022095143586](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079395.png)



## RAID

- **RAID: Redundant Arrays of Independent Disks**
- disk organization techniques that manage a large numbers of disks, providing a view of a **single** disk of
  - **high capacity** and **high speed** by using multiple disks in **parallel**
  - **high reliability** by storing data **redundantly**, so that data can be recovered even if a disk fails



### Improvement of Reliability via Redundancy

- The chance that some disk out of a **set** of *N* disks will fail is much **higher** than the chance that a **specific** single disk will fail
- **Redundancy**: store extra information that can be used to rebuild information lost in a disk failure
- **Mirroring** (**Shadowing**)
  - **Duplicate** every disk. Logical disk consists of two physical disks
  - Every **write** is carried out on both disks
  - **Reads** can take place from either disk
  - **Data loss** would occur only if a disk fails, and its mirror disk also fails before the system is repaired
- **Mean time to data loss** depends on **mean time to failure**, 
   and **mean time to repair**



### Improvement in Performance via Parallelism

- **Load balance** multiple **small** accesses to increase **throughput**
- Parallelize **large accesses** to **reduce response time**
- **Bit-level striping**: split the bits of each byte across multiple disks. But seek/access time worse than for a single disk
- **Block-level striping**: **Requests** for different blocks can run in **parallel** if the blocks reside on different disks
- A request for a **long** sequence of blocks can utilize all disks in **parallel**



### RAID Levels

- Schemes to provide redundancy at lower cost by using **disk striping combined with parity bits**
- **RAID Level 0**: Block striping; non-redundant. Used in **high-performance** applications where data loss is not critical
- **RAID Level 1**: Mirrored disks with block striping. Offers best **write** performance. Popular for applications such as **storing log files** in a database system

![image-20211022101517132](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079391.png)

- **RAID Level 2**: Memory-Style Error-Correcting-Codes (**ECC**) with **bit** striping
- **RAID Level 3**: **Bit**-Interleaved Parity
- **RAID Level 4**: **Block**-Interleaved Parity; uses block-level striping, and keeps a parity block on a **separate** **parity disk** for corresponding blocks from *N* other disks
- **RAID Level 5**: **Block**-Interleaved **Distributed** Parity; partitions data and parity among all *N* + 1 disks, rather than storing data in *N* disks and parity in 1 disk
  - with 5 disks, parity block for *n*th set of blocks is stored on disk **(*n mod* 5) + 1**, with the data blocks stored on the other 4 disks
  - Block writes occur in **parallel** if the blocks and their parity blocks are on **different disks**
  - RAID 5 is better than RAID 4, since with RAID 4 with random writes, parity disk gets much **higher write load** than other disks

![image-20211022101929720](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079389.png)

- **RAID Level 6**: P+Q Redundancy scheme; similar to Level 5, but stores **two error correction blocks** (P, Q) instead of single parity block to guard against multiple disk failures. **Better reliability** than Level 5 **at a higher cost**. Becoming more important as **storage sizes increase**



### Choice of RAID Level

- **Factors**: Monetary cost, performance, performance during failure, performance during rebuild of failed disk
- RAID 0 is used only when data safety is not important, data can be recovered quickly from other sources



### Hardware Issues

- **Software RAID**: RAID implementations done entirely in software, with no special hardware support
- **Hardware RAID**: RAID implementations with special hardware
  - Use non-volatile RAM to record **writes** that are being executed
  - **power failure** during write can result in corrupted disk
  - **NV-RAM** helps to efficiently detected potentially corrupted blocks
  - Otherwise all blocks of disk must be read and compared with **mirror/parity block**
- **Latent failures**: data successfully written earlier gets **damaged**, can result in **data loss** even if only one disk fails
- **Data scrubbing**: continually scan for latent failures, and recover from copy/parity
- **Hot swapping**: replacement of disk while system is running, without power down. Supported by some hardware **RAID** systems, reduces **time to recovery**, and improves **availability** greatly
- Many hardware RAID systems ensure that a **single point of failure** will not stop the functioning of the system by using
  - **Redundant power** supplies with battery backup
  - Multiple **controllers** and multiple **interconnections** to guard against controller/interconnection failures



## Disk-Block Access

### Optimization of Disk-Block Access

- **Buffering:** in-memory buffer to cache disk blocks
- **Read-ahead:** Read extra blocks from a track in anticipation that they will be requested soon
- **Disk-arm-scheduling** algorithms re-order block requests so that disk arm movement is minimized (**elevator algorithm**)
- **File organization**: Allocate blocks of a file in as **contiguous** a manner as possible; Allocation in units of **extents**; Files may get **fragmented**
- **Non-volatile write buffers**