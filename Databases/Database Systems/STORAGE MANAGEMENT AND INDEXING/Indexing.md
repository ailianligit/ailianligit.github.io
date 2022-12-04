# Indexing

## Basic Concepts

- Indexing mechanisms used to **speed up** access to desired data
- **Search Key**: attribute to **set of attributes** used to look up records in a file
- An **index file** consists of records (called **index entries**) of the form

| search-key | pointer |
| ---------- | ------- |

- Two basic kinds of indices:
  - **Ordered indices:** search keys are stored in sorted order
  - **Hash indices:** search keys are distributed uniformly across “buckets” using a “hash function”

- **Index Evaluation Metrics**: Access types supported efficiently; Access time; Insertion time; Deletion time; Space overhead



## Ordered Indices

- **Clustering Index (Primary Index)**: in a sequentially ordered file, the index whose search key specifies the sequential order of the file. The search key of a primary index is **usually but not necessarily the primary key**
- **Nonclustering Index (Secondary index)**: an index whose search key specifies an order **different from the sequential order** of the file
- **Index-sequential file**: sequential file ordered on a search key, with a clustering index on the search key



### Dense & Sparse Index Files

- **Dense index**: Index record appears for **every search-key value** in the file
- **Sparse Index**: contains index records for only some search-key values. Applicable when records are **sequentially ordered** on search-key
- Sparse indices compared to dense indices:
  - Less **space** and less **maintenance overhead** for insertions and deletions
  - Generally **slower** than dense index for locating records
- Tradeoff:
  - For **clustered index**: sparse index with an index entry for every block in file, corresponding to least search-key value in the block
  - For **unclustered index**: sparse index on top of dense index (**multilevel index**)
- Nonclustered Index: Index record points to a **bucket** that contains pointers to all the actual records with that particular search-key value. Secondary indices **have to be dense**

![image-20211026174626580](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211026174626580.png)



### Clustering vs Nonclustering Indices

- Indices offer **substantial** benefits when searching for records, but indices imposes **overhead** on database modification
  - when a record is **inserted, ** **deleted or updated**, every index on the relation must be updated

- **Sequential** **scan** using **clustering** index is **efficient**, but a sequential scan using a **secondary** index is **expensive** on magnetic disk
  - Each record access may fetch a new block from **disk**



### Multilevel Index

- If index does not fit in memory, access becomes **expensive**
- Solution: treat index kept on **disk** as a **sequential file** and construct a **sparse index** on it
  - **outer index**: a sparse index of the basic index
  - **inner index**: the basic index file

![image-20211026175327365](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211026175327365.png)



### Index Update: Deletion

- If deleted record was the only record in the file with its particular search-key value, the **search-key** is deleted from the index also

- **Single-level index entry deletion:**
  - **Dense indices**: deletion of search-key is similar to **file record deletion**
  - **Sparse indices**: if an entry for the search key exists in the index, it is deleted by replacing the entry in the index with the **next search-key value** in the file (in search-key order). If the next search-key value already has an index entry, the entry is **deleted** instead of being replaced



### Index Update: Insertion

- **Single-level index insertion:**
  - Perform a lookup using the search-key value of the record to be inserted
  - **Dense indices**: if the search-key value does not appear in the index, insert it
    - Indices are maintained as **sequential files**
    - Need to create space for new entry, **overflow blocks** may be required
  - **Sparse indices**: if index stores an entry for each block of the file, no change needs to be made to the index unless **a new block is created**
    - If a new block is created, the first search-key value appearing in the new block is inserted into the index



### Indices on Multiple Keys

- **Composite search key**
  - Index on *instructor* relation on attributes (*name, ID*). Can query on just *name*, or on (*name, ID*)



## B+-Tree Index Files

- Disadvantage of indexed-sequential files:
  - **Performance** degrades as file grows, since many **overflow blocks** get created
  - **Periodic reorganization** of entire file is required

- Advantage of B+-tree index files:
  - **Automatically reorganizes** itself with small, local, changes, in the face of insertions and deletions
  - Reorganization of entire file is not required to maintain performance

- (Minor) disadvantage of B+-trees: 
  - Extra insertion and deletion overhead, space **overhead**

- Advantages of B+-trees outweigh disadvantages. B+-trees are used extensively
- Since the inter-node connections are done by **pointers**, “logically” close blocks need **not be “physically” close**
- The non-leaf levels of the B+-tree form a **hierarchy of sparse indices**
- Insertions and deletions to the main file can be handled efficiently, as the index can be restructured in **logarithmic** time



### B+-Tree Structure

- A B+-tree is a rooted tree satisfying the following properties:
  - All paths from root to leaf are of the same length
  - Each node that is not a root or a leaf has between *n*/2(上界) and *n* children
  - A leaf node has between (*n*–1)/2(上界) and *n*–1 values
  - If the root is not a leaf, it has at least 2 children; If the root is a leaf (that is, there are no other nodes in the tree), it can have between 0 and (*n*–1) values
- Pi are pointers to **children** (for non-leaf nodes) or pointers to **records** or **buckets of records** (for leaf nodes)
- For a leaf node, Pn points to **next leaf node** in search-key order; For a non-leaf node, all the search-keys in the subtree to which *Pn* points have values **greater than or equal to** *Kn*–1
- The search-keys in a node are ordered: *K*1 < *K*2 < *K*3 < *. . .* < *K*n*–*1

![image-20211102111335424](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102111335424.png)

![image-20211102110207162](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102110207162.png)



### Queries on B+-Trees

```
function find(v)
	C=root
	while (C is not a leaf node)
		Let i be least number s.t. V ≤ Ki
		if there is no such number i then 
     		Set C = last non-null pointer in C 
		else if (v = C.Ki) Set C = Pi +1  
		else set C = C.Pi
	if for some i, Ki = V  then return C.Pi
	else return null /* no record with search-key value v exists. */
```

- **Range queries** find all records with search key values in a given range
- If there are *K* search-key values in the file, the height of the tree is no more than $$\lceil log_{\lceil n/2\rceil}(K)\rceil$$
- A **node** is generally the same size as a **disk block**



### Updates on B+-Trees: Insertion

- Find the leaf node in which the search-key value would appear
  - If there is room in the leaf node, insert (v, *pr*) pair in the leaf node
  - Otherwise, split the node (along with the new (*v,* *pr*) entry), and propagate updates to parent nodes
    - take the *n* (search-key value, pointer) pairs (including the one being inserted) in sorted order. Place the **first $$\lceil n/2 \rceil$$** in the original node, and the **rest** in a new node
    - let the new node be *p,* and let *k* be the least key value in *p.* Insert (*k,p*) in the **parent** of the node being split
    - If the parent is full, split it and **propagate** the split further up. Splitting of nodes proceeds upwards till a node that is not full is found
    - In the worst case the root node may be split increasing the **height** of the tree by 1
- add "Adams" & "Lamport"

![image-20211102135316942](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102135316942.png)

![image-20211102135329790](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102135329790.png)

![image-20211102135414874](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102135414874.png)

- Splitting a non-leaf node: when inserting (k,p) into an already full internal node N
  - Copy N to an in-memory area M with space for n+1 pointers and n keys
  - Insert (k,p) into M
  - Copy P1,K1, …, K$$_{\lceil n/2 \rceil -1}$$,P$$_{\lceil n/2 \rceil}$$ from M back into node N
  - Copy P$$_{\lceil n/2 \rceil +1}$$, K$$_{\lceil n/2 \rceil +1}$$, …, K$$_n$$, P$$_{n+1}$$ from M into newly allocated node N'
  - Insert (K$$_{\lceil n/2 \rceil}$$,N') into parent N



### Updates on B+-Trees: Deletion

- Assume record already deleted from file. Let *V* be the search key value of the record, and *Pr* be the pointer to the record
  - Remove (*Pr, V*) from the leaf node
  - If the node has too few entries due to the removal, and the entries in the node and a sibling fit into a single node, then **merge siblings**:
    - Insert all the search-key values in the two nodes into a single node (the one on the left), and delete the other node
    - Delete the pair (*Ki–*1, *Pi*), where *Pi* is the pointer to the deleted node, **from its parent**, recursively using the above procedure
  - Otherwise, if the node has too few entries due to the removal, but the entries in the node and a sibling do not fit into a single node, then **redistribute pointers**:
    - Redistribute the pointers between the node and a sibling such that both have more than the minimum number of entries
    - Update the corresponding search-key value in the parent of the node

- The node deletions may cascade upwards till a node which has **$$\lceil n/2 \rceil$$ or more** pointers is found
- If the **root** node has **only one pointer** after deletion, it is deleted and the sole child becomes the root

- delete "Srinivasan", "Singh", "Wu" & "Gold"

![image-20211102140407505](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102140407505.png)

![image-20211102140441131](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102140441131.png)

![image-20211102140507892](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102140507892.png)

- Value separating two nodes (at the parent) is **pulled down** when merging

![](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102140557962.png)



### Complexity of Updates

- Cost (in terms of number of I/O operations) of insertion and deletion of a single entry proportional to **height** of the tree
  - •With K entries and maximum fanout of n, worst case complexity of insert/delete of an entry is $$O(log_{\lceil n/2\rceil}(K))$$

- In practice, number of I/O operations is less:
  - Internal nodes tend to be **in buffer**
  - **Splits/merges are rare**, most insert/delete operations only affect a leaf node

- Average node occupancy depends on insertion order: 2/3rds with random, ½ with insertion in sorted order



### Non-Unique Search Keys

- If a search key *ai*  is not unique, create instead an index on a composite key **(*ai* , *Ap*)**, which is unique. *Ap* could be a primary key, record ID, or any other attribute that **guarantees uniqueness**
- Search for *ai* *= v* can be implemented by a range search on composite key, with **range (*v,* *-∞) to (*v, +∞)**
- But **more I/O operations** are needed to fetch the actual records
  - If the index is clustering, all accesses are sequential
  - If the index is non-clustering, each record access may need an I/O operation
- Alternatives to scheme described earlier
  - Buckets on separate block (bad idea)
  - **List** of tuple pointers with each key
    - Extra code to handle long lists
    - Deletion of a tuple can be expensive if there are many duplicates on search key: Worst case complexity may be linear!
    - Low space overhead, no extra cost for queries
  - Make search key unique by adding a **record-identifier**
    - Extra storage overhead for keys
    - Simpler code for insertion/deletion
    - Widely used



### B+-Tree File Organization

- B+-Tree File Organization:
  - Leaf nodes in a B+-tree file organization store **records**, instead of pointers
  - Helps keep data records **clustered** even when there are insertions/deletions/updates

- Leaf nodes are still required to be **half full**. Since records are larger than pointers, **the maximum number of records** that can be stored in a leaf node is less than **the number of pointers** in a nonleaf node
- Good space utilization important since records use more space than pointers
- To improve space utilization, involve more sibling nodes in redistribution during splits and merges
  - Involving 2 siblings in redistribution (to avoid split / merge where possible) results in each node having at least $$\lfloor 2n/3\rfloor$$ entries

![image-20211102151429742](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102151429742.png)



### Record relocation and secondary indices

- If a record moves, all **secondary indices** that store record **pointers** have to be updated
- Node **splits** in B+-tree file organizations become very expensive
- *Solution*: use **search key** of B+-tree file organization instead of record pointer in secondary index
  - Add **record-id** if B+-tree file organization search key is **non-unique**
  - Extra traversal of file organization to locate record, **higher cost for queries, but node splits are cheap**



### Indexing Strings

- Variable length strings as keys
  - Variable fanout
  - Use **space utilization** as criterion for splitting, not number of pointers
- **Prefix compression**
  - Key values at internal nodes can be **prefixes** of full key
  - Keep enough characters to **distinguish** entries in the subtrees separated by the key value
  - Keys in **leaf node** can be **compressed** by sharing common prefixes



### Bulk Loading and Bottom-Up Build

- Inserting entries one-at-a-time into a B+-tree requires ≥ 1 IO per entry
  - assuming leaf level does not fit in memory
  - can be very inefficient for loading a large number of entries at a time (**bulk loading**)

- Efficient alternative 1:
  - **sort** entries first (using efficient external-memory sort algorithms)
  - **insert in sorted order**
    - insertion will go to existing page (or cause a split)
    - much improved IO performance, but most leaf nodes half full

- Efficient alternative 2: **Bottom-up B+-tree construction**
  - As before sort entries
  - And then create tree **layer-by-layer**, starting with leaf level



### Indexing on Flash

- Random I/O **cost** much **lower** on flash
- Writes are **not in-place**, and (eventually) require a more **expensive** **erase**
- **Optimum page size** therefore much **smaller**
- **Bulk-loading** still useful since it **minimizes page erases**
- Write-optimized tree structures have been adapted to minimize page writes for flash-optimized search trees



### Indexing in Main Memory

- Random access in memory
  - Much **cheaper** than on **disk/flash**
  - But still **expensive** compared to **cache** read
  - **Data structures** that make best use of **cache** preferable
  - Binary search for a key value within a large B+-tree node results in many **cache misses**
- **B+- trees with small nodes** that fit in cache line are preferable to reduce cache misses
- Key idea: use **large node size** to optimize **disk access**, but **structure data within a node** using **a tree with small node size**, instead of using an array



## B-Tree Index Files

- Similar to B+-tree, but B-tree allows search-key values to appear **only once**; eliminates redundant storage of search keys
- Search keys in nonleaf nodes appear nowhere else in the B-tree; an **additional pointer field** for each search key in a nonleaf node must be included
- Nonleaf node: pointers Bi are the **bucket** or **file record pointers**

![image-20211102160724493](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102160724493.png)

- Advantages of B-Tree indices:
  - May use **less tree nodes** than a corresponding B+-Tree
  - Sometimes possible to find search-key value before reaching leaf node (**fast**)

- Disadvantages of B-Tree indices:
  - Only **small fraction** of all search-key values are found early 
  - Non-leaf nodes are **larger**, so **fan-out is reduced**. Thus, B-Trees typically have **greater depth** than corresponding B+-Tree
  - Insertion and deletion more **complicated** than in B+-Tree
  - Implementation is **harder** than B+-Trees

![image-20211102161015884](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102161015884.png)



## Hash Indices

### Static Hashing

- A **bucket** is a unit of storage containing one or more entries (a bucket is typically a **disk block**), we obtain the bucket of an entry from its search-key value using a **hash** **function**
- Hash function *h* is a function from **the set of all search-key values *K*** to **the set of all bucket addresses *B***
- Hash function is used to **locate entries** for access, insertion as well as deletion
- Entries with different search-key values may be mapped to the same bucket; thus entire bucket has to be searched **sequentially** to locate an entry
- In a **hash index**, buckets store **entries with pointers** to records
- In a **hash file-organization** buckets store **records**



### Handling of Bucket Overflows

- Bucket overflow can occur because of
  - **Insufficient buckets**
  - **Skew in distribution of records**. This can occur due to two reasons:
    - multiple records have same search-key value
    - chosen hash function produces non-uniform distribution of key values
- Although the probability of bucket overflow can be reduced, it cannot be eliminated; it is handled by using **overflow buckets**
- **Overflow chaining**: the overflow buckets of a given bucket are chained together in a linked list. This scheme is called **closed addressing** (also called closed hashing or open hashing)
- An alternative, called **open addressing** (also called open hashing or closed hashing) which does not use over-flow buckets, is not suitable for database applications



### Deficiencies of Static Hashing

- In static hashing, function *h* maps search-key values to a fixed set of *B* of bucket addresses. Databases **grow or shrink with time**
  - If initial number of buckets is too small, and file grows, **performance will degrade** due to **too much overflows**
  - If space is allocated for anticipated growth, a significant amount of space will be **wasted initially** (and buckets will be underfull)
  - If database shrinks, again **space** will be **wasted**
- One solution: **periodic re-organization** of the file with a **new hash function**. Expensive, disrupts normal operations
- Better solution: allow the **number of buckets** to be **modified dynamically**



### Dynamic Hashing

- Periodic rehashing
  - If number of entries in a hash table becomes (say) **1.5** times size of hash table, create **new** hash table of size (say) 2 times the size of the previous hash table and **rehash** all entries to new table
- Linear Hashing: Do rehashing in an **incremental** manner
- Extendable Hashing
  - **Tailored** to **disk** based hashing, with buckets **shared** by multiple hash values
  - Doubling of # of **entries** in hash table, without doubling # of **buckets**



### Comparison of Ordered Indexing and Hashing

- Cost of periodic re-organization
- Relative frequency of insertions and deletions
- Is it desirable to optimize average access time at the expense of worst-case access time?
- Expected type of queries:
  - Hashing is generally better at retrieving records having a **specified value of the key**
  - If **range** queries are common, ordered indices are to be preferred



### Indices on Multiple Attributes

- Suppose we have an index on combined search-key (*dept_name, salary*)

- With the **where** clause
        where *dept_name =* “Finance” and *salary =* 80000

  the index on (*dept_name, salary*) can be used to fetch only records that satisfy **both** conditions

  Using separate indices in less efficient: we may fetch many records (or pointers) that **satisfy only one** of the conditions

- Can also efficiently handle	

  ​	where *dept_name* = “Finance” and *salary* < 80000

- But cannot efficiently handle, may fetch many records that **satisfy the first but not the second condition**
  	where *dept_name* < “Finance” and *balance =* 80000



## Bitmap Indices

- Bitmap indices are a special type of index designed for **efficient querying on multiple keys**
- Records in a relation are assumed to be numbered **sequentially** from, say, 0. Given a number *n* it must be easy to retrieve record *n*. Particularly easy if records are of fixed size
- Applicable on attributes that take on a **relatively small number of distinct values**
- A bitmap is simply an array of 0-1 bits
- In its simplest form a bitmap index on an attribute has a bitmap for each value of the attribute
  - Bitmap has as many bits as **records**
  - In a bitmap for value v, the bit for a record is 1 if the record has the value v for the attribute, and is 0 otherwise

![image-20211102165938047](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211102165938047.png)

- Bitmap indices are useful for **queries on multiple attributes**, not particularly useful for **single attribute queries**

- Queries are answered using bitmap operations: Intersection (and), Union (or). Each operation takes two bitmaps of the same size and applies the operation on corresponding bits to get the result bitmap

- Can then retrieve **required tuples**. Counting **number** of matching tuples is even faster
- Bitmap indices generally very **small** compared with relation size



### Efficient Implementation of Bitmap Operations

- Bitmaps are packed into **words**; a single word and (a basic CPU instruction) computes and of 32 or 64 bits at once
- Counting number of 1s can be done fast by a trick:
  - Use each **byte** to index into a precomputed array of 256 elements each storing the count of 1s in the binary representation
  - Can use pairs of bytes to speed up further at a higher memory cost
  - Add up the retrieved counts



## Explanation

- 索引的目的、索引文件、搜索码、索引的类型1
- 顺序索引、索引顺序文件和主索引、辅助索引5
- 多级索引12
- B+树结构19
- **B+树查询算法**25
- **B+树插入算法**31
- **B+树删除算法**34
