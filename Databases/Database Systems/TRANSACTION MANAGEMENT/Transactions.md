# Transactions

## Transaction Concept

- A **transaction** is a *unit* of program execution that accesses and possibly updates various data items
- **Atomicity requirement**: Either **all** operations of the transaction are properly reflected in the database or none are
- **Consistency requirement**: Explicitly specified integrity constraints such as primary keys and foreign keys & implicit integrity constraints. Execution of a transaction in **isolation** preserves the consistency of the database
- **Isolation requirement**: Although multiple transactions may execute concurrently, each transaction must be **unaware** of other concurrently executing transactions. **Intermediate transaction results** must be hidden from other concurrently executed transactions
- **Durability requirement**: Once the user has been notified that the transaction has completed, the updates to the database by the transaction must persist even if there are software or hardware failures



## Transaction State

- **Active**: the initial state; the transaction stays in this state while it is executing
- **Partially committed**: after the final statement has been executed

- **Failed**: after the discovery that normal execution can no longer proceed

- **Aborted**: after the transaction has been rolled back and the database restored to its state prior to the start of the transaction. Two options after it has been aborted:
  - Restart the transaction
  - Kill the transaction
- **Committed**: after successful completion

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211109163901235.png" alt="image-20211109163901235" style="zoom:50%;" />



## Concurrent Executions

- **Increased processor and disk utilization**, leading to better transaction *throughput*
- **Reduced average response time** for transactions: short transactions need not wait behind long ones
- **Concurrency control schemes**: mechanisms to achieve isolation
  - to control the interaction among the concurrent transactions in order to prevent them from destroying the **consistency** of the database
- **Schedule**: a sequences of instructions that specify the **chronological** order in which instructions of concurrent transactions are executed
  - A schedule for a set of transactions must consist of **all** instructions of those transactions
  - Must preserve the **order** in which the instructions appear in each individual transaction

- A transaction that successfully completes its execution will have a **commit** instructions as the last statement
- A transaction that fails to successfully complete its execution will have an **abort** instruction as the last statement



## Serializability

- **Basic Assumption**: Each transaction preserves database consistency
- **Serial** execution of a set of transactions preserves database **consistency**. A (possibly concurrent) schedule is serializable if it is equivalent to a serial schedule



### Conflicting Serializability

- If *li* and *lj* are consecutive in a schedule and they do not conflict, their results would remain the **same** even if they had been **interchanged** in the schedule
- If a schedule *S* can be transformed into a schedule *S’* by a series of swaps of **non-conflicting instructions**, we say that *S* and *S’* are **conflict equivalent**
- We say that a schedule *S* is **conflict serializable** if it is conflict equivalent to a **serial** schedule



### View Serializability

- Let *S* and *S’* be two schedules with the same set of transactions. *S* and *S’* are **view equivalent** if the following three conditions are met, for each data item *Q*
  - If in schedule S, transaction *Ti* reads the initial value of *Q*, then in schedule *S’* also transaction *Ti*  must read the initial value of *Q*
  - If in schedule S transaction *Ti* executes **read**(*Q)*, and that value was produced by transaction *Tj* (if any), then in schedule *S’* also transaction *Ti* must read the value of *Q* that was produced by the same **write**(Q) operation of transaction *Tj*
  - The transaction (if any) that performs the final **write**(*Q*) operation in schedule *S* must also perform the final **write**(*Q*) operation in schedule *S'*
- A schedule *S* is **view serializable** if it is view equivalent to a serial schedule
- Every conflict serializable schedule is also view serializable
- Every view serializable schedule that is not conflict serializable has **blind writes**



### Test for Conflict Serializability

- A schedule is conflict serializable if and only if its precedence graph is **acyclic**
- If precedence graph is acyclic, the serializability order can be obtained by a ***topological sorting*** of the graph
- This is a **linear** order consistent with the partial order of the graph



### Test for View Serializability

- The precedence graph test for conflict serializability **cannot** be used directly to test for view serializability
  - Extension to test for view serializability has cost **exponential** in the size of the precedence graph
- The problem of checking if a schedule is view serializable falls in the class of ***NP*-complete** problems

- However practical algorithms that just check some **sufficient** **conditions** for view serializability can still be used



## Transaction Isolation & Atomicity

### Recoverable Schedules

- **Recoverable** **schedule**: if a transaction *Tj* reads a data item previously written by a transaction *Ti*, then the **commit** operation of *Ti* appears **before** the **commit** operation of *Tj*
- The following schedule is not recoverable:

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211116163536545.png" alt="image-20211116163536545" style="zoom: 67%;" />



### Cascadeless Schedules

- **Cascading rollback**: a single transaction failure leads to a series of transaction rollbacks. Can lead to the undoing of a significant amount of work
- The schedule is recoverable:

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211116164008699.png" alt="image-20211116164008699" style="zoom:67%;" />

- **Cascadeless** **schedules**: cascading rollbacks cannot occur; For each pair of transactions *Ti* and *Tj* such that *Tj* reads a data item previously written by *Ti*, the **commit** operation of *Ti*  appears **before** the **read** operation of *Tj*



## Transaction Isolation Levels

### Concurrency Control

- A database must provide a mechanism that will ensure that all possible schedules are either conflict or view **serializable**, and are **recoverable** and **preferably cascadeless**
- **Goal**: to develop concurrency control protocols that will assure **serializability**
- Concurrency-control schemes tradeoff between **the amount of concurrency** they allow and the amount of **overhead** that they incur



### Levels of Consistency

- **Serializable**: default
- **Repeatable read**: only committed records to be read
  - Repeated reads of same record must return same value
- **Read committed**: only committed records can be read
  - Successive reads of record may return different (but committed) values
- **Read uncommitted**: even uncommitted records may be read
- Lower degrees of consistency useful for gathering **approximate**
   information about the database



## Implementation of Isolation Levels

- **Locking**

- **Timestamps**
  - Transaction timestamp assigned e.g. when a transaction **begins**
  - Data items store two timestamps: Read timestamp & Write timestamp
  - Timestamps are used to detect out of order accesses

- Multiple versions of each data item
  - Allow transactions to read from a “**snapshot**” of the database



## Transactions as SQL Statements

- **phantom phenomenon**



## Explanation

- 事务2
- ACID特性7
- 事务状态8

- 调度11
- 串行保证数据库一致性16
- 冲突等价和冲突可串行19
- 视图等价和视图可串行22
- 冲突可串行的调度视图可串行
- 没有冲突可串行的视图可串行存在盲写
- 冲突可串行的检测26
- 可恢复调度28
- 级联回滚29
- 无级联调度30
