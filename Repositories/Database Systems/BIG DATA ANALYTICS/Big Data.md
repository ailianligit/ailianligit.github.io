# Big Data

## Motivation

- **Volume**: much larger amounts of data stored
- **Velocity**: much higher rates of insertions
- **Variety**: many types of data, beyond relational data



## Big Data Storage System

### Distributed File Systems

- A distributed file system stores data across a large collection of machines, but provides single file-system view

- Highly **scalable** distributed file system for large data-intensive applications

- Provides **redundant** storage of massive amounts of data on cheap and unreliable computers
  - Files are replicated to handle hardware failure
  - Detect failures and recovers from them
- Examples:
  - Google File System (GFS)
  - Hadoop File System (HDFS)
- **Hadoop File System Architecture**:
  - Single **Namespace** for entire cluster
  - Files are broken up into **blocks**. Typically 64 MB block size. Each block replicated on multiple DataNodes
  - **Client** finds location of blocks from NameNode and accesses data directly from DataNode
  - **NameNode**
    - Maps a **filename** to list of **Block IDs**
    - Maps each **Block ID** to **DataNodes** containing a **replica** of the block
  - **DataNode**: Maps a **Block ID** to a physical location on disk
  - Data Coherency
    - Write-once-read-many access model
    - Client can only append to existing files
- Distributed file systems good for **millions** of large files
- But have very high **overheads** and poor **performance** with **billions** of smaller tuples

<img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079100.png" alt="image-20211214103327879" style="zoom:67%;" />



### Sharding

- **Sharding**: partition data across **multiple** databases
- Partitioning usually done on some **partitioning attributes** (also known as **partitioning keys** or **shard keys** e.g. user ID
  - E.g., records with key values from 1 to 100,000 on database 1, records with key values from 100,001 to 200,000 on database 2, etc
- Application must **track** which records are on which database and send queries/updates to that database
- Positives: **scales** well, easy to **implement**
- Drawbacks:
  - Not **transparent**: application has to deal with routing of queries, queries that span multiple databases
  - When a database is **overloaded**, moving part of its load out is not easy
  - Chance of **failure** more with more databases
  - need to keep **replicas** to ensure **availability**, which is more work for application



### Key Value Storage Systems

- Key-value storage systems store large numbers (billions or even more) of **small** (KB-MB) sized **records**
- Records are partitioned across multiple **machines** and **Queries** are **routed** by the system to appropriate machine
- Records are also **replicated** across multiple **machines**, to ensure **availability** even if a machine fails
- Key-value stores ensure that **updates** are applied to **all replicas**, to ensure that their values are **consistent**
- Document stores store semi-structured data, typically **JSON**
- a JSON object

```json
{
    "ID": "22222",
    "name": {
           "firstname: "Albert",
           "lastname: "Einstein"
    },
    "deptname": "Physics",
    "children": [
           { "firstname": "Hans", "lastname": "Einstein" },
           { "firstname": "Eduard", "lastname": "Einstein" }
    ]
}
```

- Key value stores are **not full** database systems
  - Have no/limited support for **transactional** **updates**
  - Applications must manage **query** **processing** on their own
- Not supporting above features makes it easier to build scalable data storage systems
- Also called **NoSQL** systems



### Parallel and Distributed Databases

- Parallel databases run multiple machines (**cluser**)
- Parallel databases were designed for **smaller** **scale** (10s to 100s of machines)
- Did **not** provide easy **scalability**
- **Replication** used to ensure data availability despite machine failure
- But typically **restart** query in event of failure. Restarts may be frequent at very large scale



### Replication and Consistency

- **Availability** (system can run even if parts have failed) is essential for **parallel**/**distributed** databases
  - Via replication, so even if a node has failed, another copy is available
- **Consistency** is important for replicated data
  - All live replicas have same value, and each read sees latest version



## The MapReduce Paradigm

- Platform for **reliable**, **scalable** **parallel** computing

- **Abstracts** issues of distributed and parallel environment from programmer

  - Programmer provides core **logic** (via map() and reduce() functions)

  - System takes care of **parallelization** of computation, coordination, etc

- Data storage/access typically done using **distributed** file systems or **key-value** stores



### MapReduce Programming Model

- Input: a set of **key/value pairs**
- User supplies two functions:
  - **map**(k,v) -> list(k1,v1)
  - **reduce**(k1, list(v1)) -> v2

<img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079104.png" alt="image-20211214111323987" style="zoom: 67%;" />



### Word Count Example

- **Divide** **documents** among workers
- Each worker parses document to find all words, **map** **function** outputs (word, count) pairs
- Partition (word, count) pairs across workers based on **word**
- For each word at a worker, **reduce** **function** locally add up counts

- First attribute of emit above is called **reduce key**

- In effect, group by is performed on reduce key to create a list of values (all 1’s in above code)

- This requires **shuffle step** across machines

- The reduce function is called on list of **values** in each group



### Parallel Processing of MapReduce Job

<img src="https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691079106.png" alt="image-20211214111427385" style="zoom:80%;" />



