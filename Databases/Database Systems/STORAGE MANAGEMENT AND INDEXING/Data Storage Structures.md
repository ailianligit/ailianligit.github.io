# Data Storage Structures

## File Organization

- The **database** is stored as a collection of **files**
- Each **file** is a sequence of **records**
- A **record** is a sequence of **fields**
- **records** are smaller than a disk **block**



### Fixed-Length Records

- Store record *i* starting from byte *n* * (*i* *–* 1), where *n* is the size of each **record**
- Record access is simple but records may **cross blocks**. Modification: do not allow records to cross block boundaries
- Deletion of record *i:* alternatives:
  - move records *i* + 1, . . ., *n* to *i*, . . . , *n* – 1
  - move record *n* to *i*
  - do not move records, but link all free records on a ***free list***



### Variable-Length Records

- Variable-length records arise in database systems in several ways:
  - Storage of **multiple record types** in a file
  - Record types that allow **variable lengths** for one or more **fields** such as strings (varchar)
  - Record types that allow **repeating fields** (used in some older data models)
- **Attributes** are stored **in order**
- Variable length attributes represented by **fixed size** (offset, length), with actual data stored after all fixed length attributes
- Null values represented by **null-value bitmap**

![image-20211022103958710](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211022103958710.png)

- **Slotted page** **header** contains: number of record entries, end of free space in the block, location and size of each record

![image-20211022104040835](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211022104040835.png)

- Records can be moved around **within a page** to keep them contiguous with **no empty space** between them; entry in the header must be updated
- Pointers should not point directly to record, instead they should point to the **entry** for the record in header



### Storing Large Objects

- **Records** must be smaller than **pages**
- Store as files in **file systems**; Store as files managed by **database**; Break into pieces and store in **multiple tuples** in **separate relation**



## Organization of Records in Files

- **Heap**: record can be placed **anywhere** in the file where there is space
- **Sequential**: store records in **sequential order**, based on the **value of the search key** of each record
- In a **multitable** **clustering** file organization records of several **different relations** can be stored in the **same file**
  - Motivation: store related records on the same block to minimize I/O

- **B+-tree** file organization: **Ordered** storage even with **inserts/deletes**
- **Hashing**: a hash function computed on **search key**; the result specifies in which block of the file the record should be placed



### Heap File Organization

- Records can be placed **anywhere** in the file where there is free space
- Records usually **do not move** once allocated
- Important to be able to **efficiently** find **free space** within file
- **Free-space map**: Can have **second-level** free-space map
- Free space map written to disk periodically, OK to have wrong (old) values for some entries



### Sequential File Organization

- Suitable for applications that require **sequential processing** of the entire file
- The records in the file are ordered by a **search-key**
- **Deletion**: use pointer chains
- **Insertion**: locate the position where the record is to be inserted
  - if there is free space insert there
  - if no free space, insert the record in an **overflow block**
  - In either case, pointer chain must be updated
- Need to **reorganize** the file **from time to time** to restore sequential order



### Multitable Clustering File Organization

![image-20211022105626413](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211022105626413.png)

- good for queries involving ***department* ⨝ *instructor***, and for queries involving one single department and its instructors
- bad for queries involving **only** *department*
- results in **variable size records**
- Can add **pointer chains** to link records of a particular relation



### Partitioning

- **Table partitioning**: Records in a relation can be partitioned into smaller relations that are stored separately
- Queries written on ***transaction*** must access records in **all partitions**, unless query has a selection such as *year=*2019, in which case only one partition in needed
- Reduces **costs** of some operations such as **free space management**
- Allows different partitions to be stored **on different storage devices** 



## Data-Dictionary Storage

- The **Data dictionary** (system catalog) stores **metadata**; that is, data about data, such as 
  - **Information about relations**: names of relations, names, types and lengths of attributes of each relation, names and definitions of views & integrity constraints
  - **User and accounting information**, including passwords
  - **Statistical and descriptive data**: number of tuples in each relation
  - **Physical file organization information**: How relation is stored (sequential/hash/…) & Physical location of relation
  - **Information about indices**
- Relational Representation on disk of System Metadata

![image-20211022110252800](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211022110252800.png)



## Database Buffer

### Storage Access

- Blocks are units of both **storage allocation** and **data transfer**
- Database system seeks to minimize the number of block transfers between the disk and memory. We can reduce the number of disk accesses by keeping as many blocks as possible in **main memory**
- **Buffer**: portion of main memory available to store copies of disk blocks
- **Buffer manager**: subsystem responsible for allocating buffer space in main memory

![image-20211026164807299](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211026164807299.png)



### Buffer Manager

- Programs call on the **buffer manager** when they need a block from disk

  - If the block is already in the buffer, buffer manager returns the address of the block in main memory
  - If the block is not in the buffer, the buffer manager allocates space in the buffer for the block, reads the block from the disk to the buffer, and returns the address of the block in main memory to requester

  - **Replacing** (throwing out) some other block, if required, to make space for the new block. Replaced block written back to disk only if it was **modified** since the most recent time that it was written to/fetched from the disk

- **Buffer replacement strategy**
- **Pinned block:** memory block that is not allowed to be written back to disk
  - **Pin** done before reading/writing data from a block
  - **Unpin** done when read /write is complete
  - Multiple **concurrent** pin/unpin operations possible, keep a pin count, buffer block can be evicted only if pin count = 0

- **Shared and exclusive locks on buffer**
  - Needed to prevent concurrent operations from reading page contents as they are moved/reorganized, and to ensure only one move/reorganize at a time
  - Readers get **shared lock**, updates to a block require **exclusive lock**
  - **Locking rules:** Only one process can get exclusive lock at a time; Shared lock cannot be concurrently with exclusive lock; Multiple processes may be given shared lock concurrently



### Buffer-Replacement Policies

- Most operating systems replace the block **least recently used** (LRU strategy)
- **Toss-immediate** strategy: frees the space occupied by a block as soon as the final tuple of that **block** has been processed
- **Most recently used (MRU) strategy**: system must **pin the block** currently being processed. After the final tuple of that **block** has been processed, the block is unpinned, and it becomes the most recently used block
- **Queries** have well-defined access patterns (such as sequential scans), and a database system can use the information in a user’s query to predict future references
- **Mixed** strategy with hints on replacement strategy provided by the query optimizer is preferable
- Buffer manager can use statistical information regarding the probability that **a request will reference a particular relation**
- Operating system or buffer manager may **reorder writes**
  - Can lead to **corruption** of data structures on disk (E.g., linked list of blocks with **missing block** on disk. File systems perform **consistency** check to detect such situations)



### Optimization of Disk Block Access

- Buffer managers support **forced output** of blocks for the purpose of **recovery**
- **Nonvolatile write buffers** speed up disk writes by writing blocks to a **non-volatile RAM or flash buffer** immediately. Writes can be **reordered** to minimize disk arm movement
- **Log disk**: a disk devoted to writing a sequential log of **block updates**. Used exactly like nonvolatile RAM. Write to log disk is very **fast** since no seeks are required
- **Journaling file systems** write data in-order to **NV-RAM or log disk**. Reordering without journaling: risk of **corruption** of file system data



## Column-Oriented Storage

- Column-Oriented Storage also known as **columnar** **representation**
- Store each **attribute** of a relation separately
- Benefits:
  - **Reduced IO** if only some attributes are accessed
  - **Improved CPU cache performance**
  - **Improved compression**
  - **Vector processing** on modern CPU architectures

- Drawbacks:
  - Cost of tuple **reconstruction** from columnar representation
  - Cost of tuple **deletion and update**
  - Cost of **decompression**
- Columnar representation found to be more efficient for **decision support** than row-oriented representation. Traditional row-oriented representation preferable for **transaction processing**
- Some databases support both representations. Called **hybrid row/column stores**
- **ORC and Parquet**: file formats with columnar storage inside file. Very popular for **big-data** applications

![image-20211026172236090](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211026172236090.png)



## Storage Organization in Main-Memory Databases

- Can store records directly in memory **without a buffer manager**
- **Column-oriented storage** can be used in-memory for decision support applications. **Compression** reduces memory requirement

![image-20211026171735431](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211026171735431.png)