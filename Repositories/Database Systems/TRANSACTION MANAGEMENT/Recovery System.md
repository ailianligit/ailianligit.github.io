# Recovery System

## Failure Classification

- **Transaction failure**:
  - **Logical errors**: transaction cannot complete due to some internal error condition
  - **System errors**: the database system must terminate an active transaction due to an error condition (e.g., deadlock)
- **System crash**: a power failure or other hardware or software failure causes the system to crash
  - **Fail-stop assumption**: non-volatile storage contents are assumed to not be corrupted by system crash. Database systems have numerous integrity checks to prevent corruption of disk data
- **Disk failure**: a head crash or similar disk failure destroys all or part of disk storage
  - Destruction is assumed to be detectable: disk drives use **checksums** to detect failures
- **Recovery algorithms** have two parts
  - Actions taken during normal transaction processing to ensure enough information exists to recover from failures
  - Actions taken after a failure to recover the database contents to a state that ensures atomicity, consistency and durability



## Storage

- **Volatile storage**: Does not survive **system crashes**
  - Examples: main memory, cache memory
- **Nonvolatile storage**: Survives **system crashes**. But may still **fail**, losing data
  - Examples: disk, tape, flash memory, non-volatile RAM 

- **Stable storage**: A mythical form of storage that survives all failures
  - Approximated by maintaining multiple copies on distinct nonvolatile media



### Stable-Storage Implementation

- Failure during data transfer can still result in inconsistent copies: Block transfer can result in
  - Successful completion
  - **Partial failure**: destination block has incorrect information
  - **Total failure**: destination block was never updated
- Protecting storage media from failure during data transfer (one solution). Execute output operation as follows (assuming two copies of each block):
  - Write the information onto the **first** physical block
  - When the first write successfully completes, write the same information onto the **second** physical block
  - The output is completed only after the second write successfully completes

- Copies of a block may differ due to failure during output operation. To recover from failure:
  - First find inconsistent blocks:
    - Record in-progress disk writes on non-volatile storage (Flash, Non-volatile RAM or special area of disk)
    - Use this information during recovery to find blocks that may be **inconsistent**, and only compare copies of these
    - Used in hardware **RAID** systems

•If either copy of an inconsistent block is detected to have an error 

   (bad checksum), overwrite it by the other copy. If both have no error, 

   but are different, overwrite the second block by the first block



### Data Access

- **Physical blocks** are those blocks residing on the disk
- **Buffer blocks** are the blocks residing temporarily in main memory
- Block movements between disk and main memory are initiated through the following two operations:
  - **input** (*B*) transfers the physical block *B* to main memory
  - **output** (*B*) transfers the buffer block *B* to the disk, and replaces the appropriate physical block there
- We assume, for simplicity, that each data item fits in, and is stored inside, a **single** block
- Each transaction *Ti* has its private work-area in which local copies of all data items **accessed and updated** by it are kept. *Ti* 's local copy of a data item *X* is called *xi*
- Transferring data items between system buffer blocks and its private work-area done by:
  - **read**(*X*) assigns the value of data item *X* to the local variable *xi*
  - **write**(*X*) assigns the value of local variable *xi* to data item {*X*} in the buffer block
- Transactions:
  - Must perform **read**(*X*) before accessing *X* for the **first time** (subsequent reads can be from local copy)
  - **write**(*X*) can be executed at any time **before the transaction commits**

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211208002818719.png" alt="image-20211208002818719" style="zoom: 50%;" />



## Recovery and Atomicity

### Log Records

- A **log** is a sequence of **log records**. The records keep information about update activities on the database
- The log is kept on **stable storage**
- When transaction *Ti* starts, it registers itself by writing a <*Ti* **start**> log record
- *Before* *Ti* executes **write**(*X*), a log record <*Ti*, *X*, *V1*, *V2*> is written, where *V1* is the value of *X* before the write (the **old** **value**)**,** and *V2* is the value to be written to *X* (the **new value**)
- When *Ti* finishes it last statement, the log record <*Ti* **commit**> is written
- Two approaches using logs
  - Immediate database modification
  - Deferred database modification. 



### Database Modification

- The **immediate-modification** scheme allows updates of an uncommitted transaction to be made to the **buffer**, or the **disk** itself, before the transaction commits
- Update log record must be written **before** database item is written
- We assume that the log record is output **directly** to stable storage
- Output of updated blocks to disk can take place at any time before or after transaction commit
- Order in which blocks are output can be different from the order in which they are written
- The **deferred-modification** scheme performs updates to buffer/disk only at the time of transaction commit
  - **Simplifies** some aspects of **recovery**
  - But has **overhead** of storing local copy



### Transaction Commit

- A transaction is said to have committed when its commit log record is output to stable storage
- All previous log records of the transaction must have been output already
- Writes performed by a transaction may still be in the buffer when the transaction commits, and may be output later



### Concurrency Control and Recovery

- With concurrent transactions, all transactions share a single disk **buffer** and a single **log**
- A **buffer block** can have data items updated by one or more **transactions**
- We assume that *if a transaction* *Ti* *has modified an item, no other transaction can modify the same item until* *Ti* *has committed or aborted*
  - i.e., the updates of uncommitted transactions should not be visible to other transactions
  - Can be ensured by obtaining **exclusive locks** on updated items and holding the locks till end of transaction (strict two-phase locking)



### Undo and Redo Operations

- **undo**(*T*i): restores the value of all data items updated by *Ti* to their old values, going backwards from the last log record for *Ti*
  - Each time a data item X is restored to its old value V a special log record <*Ti* *, X, V>* is written out
  - When undo of a transaction is complete, a log record 
     <*Ti* **abort**> is written out
- **redo**(*T*i): sets the value of all data items updated by *Ti*  to the new values, going forward from the first log record for *Ti*



### Recovering from Failure

- When recovering after failure:
  - Transaction *Ti* needs to be **undone** if the log contains the record <*Ti* **start**>, but does not contain either the record <*Ti* **commit**> or <*Ti* **abort**>
  - Transaction *Ti* needs to be **redone** if the log contains the records <*Ti* **start**> and contains the record <*Ti* **commit**> or <*Ti* **abort**>

- Suppose that transaction *Ti* was undone earlier and the <*Ti* **abort**> record was written to the log, and then a failure occurs, on recovery from failure transaction *Ti*  is redone. Such a **redo** redoes all the original actions of transaction *Ti* **including the steps that restored old values** (undo)
  - Known as **repeating history**
  - Seems wasteful, but simplifies recovery greatly



### Checkpoints

- Streamline recovery procedure by periodically performing **checkpointing**
  - Output all log records currently residing in main memory onto stable storage
  - Output all modified buffer blocks to the disk
  - Write a log record <**checkpoint** *L*> onto stable storage where *L* is a list of all transactions active at the time of checkpoint
  - All updates are stopped while doing checkpointing

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211208170906189.png" alt="image-20211208170906189" style="zoom:67%;" />



## Recovery Algorithm

- **Logging** (during normal operation):
  - <*Ti* **start**> at transaction start
  - <*Ti*, *Xj*, *V1*, *V2*> for each update, and
  - <*Ti* **commit**> at transaction end

- **Recovery from failure**: Two phases
  - **Redo phase**: replay updates of **all** transactions, whether they committed, aborted, or are incomplete
  - **Undo phase**: undo all incomplete transactions

- **Redo phase**:
  - Find last <**checkpoint** *L*> record, and set undo-list to *L*
  - Scan forward from above <**checkpoint** *L*> record
    - Whenever a record <*Ti,* *Xj*, *V1*, *V2*> or <*Ti*, *Xj*, *V2*> is found, redo it by writing *V2* to *Xj*
    - Whenever a log record <*Ti*  **start**> is found, add *Ti* to undo-list
    - Whenever a log record <*Ti* **commit**> or <*Ti* **abort**> is found, remove *Ti* from undo-list

- **Undo phase:** Scan log backwards from end
  - Whenever a log record <*Ti*, *Xj*, *V1*, *V2*> is found where *Ti* is in undo-list perform same actions as for transaction rollback:
    - perform undo by writing *V1* to *Xj*
    - write a log record <*Ti*, *Xj*, *V1*>
  - Whenever a log record <*Ti* **start**> is found where *Ti* is in undo-list,
    - Write a log record <*Ti*  **abort**>
    - Remove *Ti* from undo-list
  - Stop when undo-list is empty
    - i.e., <*Ti* **start**> has been found for every transaction in undo-list

![image-20211208172419165](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211208172419165.png)



## Explanation

- 事务故障（逻辑错误、系统错误）、系统崩溃、磁盘损坏3
- 数据访问（输入、输出、读、写）8 9
- 基于日志的恢复机制11
- 立即修改和延迟修改的定义13
- undo和redo17
- 重复历史19
- 检查点21
- 恢复算法26-28
