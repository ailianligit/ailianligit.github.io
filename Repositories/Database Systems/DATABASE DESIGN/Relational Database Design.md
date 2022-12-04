# Relational Database Design

## Features of Good Relational Design

- **lossy decomposition**

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009233206441.png" alt="image-20211009233206441" style="zoom:67%;" />

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009232929557.png" alt="image-20211009232929557" style="zoom: 67%;" />

- **lossless decomposition**: if there is no loss of information by replacing R with the two relation schemas ***R1*  U *R2***

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009233132702.png" alt="image-20211009233132702" style="zoom: 67%;" />

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009233230387.png" alt="image-20211009233230387" style="zoom:67%;" />

- **Normalization Theory**: Decide whether a particular relation *R* is in “good” form (Each relation is in **good** form; The decomposition is a **lossless** decomposition), based on **functional dependencies** & **multivalued dependencies**



## Functional Dependencies

- **legal instance**: An instance of a relation that satisfies all such **real-world** constraints
- A legal instance of a database is one where **all** the relation instances are legal instances
- **Constraints** on the set of legal relations
- Require that the value for a certain **set of attributes** determines uniquely the value for another set of attributes
- A **functional dependency** is a **generalization** of the notion of a ***key***



### Functional Dependencies Definition

- The functional dependency **α→β** holds on ***R*** if and only if for any **legal relations *r*(R)**, whenever any two tuples *t*1 and *t*2 of *r* agree on the attributes a, they also agree on the attributes *b*

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009234749456.png" alt="image-20211009234749456" style="zoom:67%;" />



### Closure of a Set of Functional Dependencies

- The set of **all** functional dependencies logically implied by *F* is the **closure** of *F* (If *A* → *B* and *B* → *C*, then we can infer that *A* → C)



## Decomposition Using Functional Dependencies

### Keys and Functional Dependencies

- *K* is a **superkey** for relation schema *R* if and only if *K* → *R*
- *K* is a **candidate** key for *R* if and only if *K* → *R*, and for no α ⫋ *K,* α → *R*



### Use of Functional Dependencies

- We use functional dependencies to
  - To test **relations** to see if they are **legal** under a given set of functional dependencies, if a relation *r* is legal under a set *F* of functional dependencies, we say that *r* **satisfies** *F*
  - To specify **constraints** on the set of legal relations, we say that *F* **holds on** *R* if all legal relations on *R* satisfy the set of functional dependencies *F*
- A specific instance of a relation schema may **satisfy** a functional dependency even if the functional dependency does **not hold on** all legal instances



### Trivial Functional Dependencies

- *A* functional dependency is **trivial** if it is satisfied by **all instances** of a relation: α → *β* is trivial if *β* ⊆ α



### Lossless Decomposition

- A decomposition of *R* into *R*1 and *R*2 is **lossless decomposition** if at least one of the following dependencies is in *F*+:
  - *R*1 ∩ *R*2 → *R*1
  - *R*1 ∩ *R*2 → *R*2
- The above functional dependencies are a **sufficient condition** for lossless join decomposition; the dependencies are a necessary condition only if **all** **constraints** are functional dependencies



### Dependency Preservation

- If testing a functional dependency can be done by considering just **one relation**, then the cost of testing this constraint is **low**
- A decomposition that makes it **computationally hard** to enforce functional dependency is said to be **NOT** dependency preserving



## Normal Forms

### Boyce-Codd Normal Form

- A relation schema *R* is in BCNF with respect to a set *F* of functional dependencies if for all functional dependencies in ***F*+** of the form α → β, where α ⊆ *R* and *β* ⊆ *R*, at least one of the following holds:
  - α → *β* is **trivial** (i.e., *β* ⊆ α)
  - α is a **superkey** for *R*



### Decomposing a Schema into BCNF

- Let R be a schema *R* that is not in BCNF. Let α → *β*  be the FD that causes a violation of BCNF
- We decompose *R* into: ( α U β ) & ( *R* - ( *β* *-* α ) )
- It is not always possible to achieve both **BCNF** and **dependency preservation**



### Third Normal Form

- A relation schema *R* is in **third normal form (3NF)** if for all: α → *β* in *F*+, at least one of the following holds:
  - α → *β* is trivial (i.e., *β* ∈ α)
  - α is a superkey for *R*
  - Each attribute *A* in *β* – α is contained in a **candidate key** for *R* (**NOTE**: each attribute may be in a **different** candidate key)
- If a relation is in BCNF it is in 3NF
- Third condition is a **minimal relaxation** of BCNF to ensure dependency preservation



### Comparison of BCNF and 3NF

- Advantages to 3NF over BCNF. It is always possible to obtain a 3NF design without sacrificing **dependency preservation**
- Disadvantages to 3NF:
  - We may have to use **null values** to represent some of the possible meaningful relationships among data items
  - There is the problem of **repetition** of information



### Goals of Normalization

- Let *R* be a relation scheme with a set *F* of functional dependencies
- Decide whether a relation scheme *R* is in **“good” form** (without repetition & null values)
- In the case that a relation scheme *R* is not in “good” form, need to **decompose** it into a set of relation scheme {*R*1*, R*2*, ..., Rn*} such that:
  - Each relation scheme is in **good form**
  - The decomposition is a **lossless decomposition**
  - Preferably, the decomposition should be **dependency preserving**



## Functional Dependency Theory

### Closure of a Set of Functional Dependencies

- F+: The set of **all** functional dependencies logically implied by *F* is the **closure** of *F*
- If *A* → *B* and *B* → *C*, then we can infer that *A* → C
- Armstrong’s Axioms:
  - **Reflexive rule:** if β ⊆ α, then α → β
  - **Augmentation rule:** if α → β, then γα →  γβ
  - **Transitivity rule:** if α → β, and β → γ, then α → γ
- Additional Rules:
  - **Union rule**: If α → β holds *a*nd α → γ holds, then α → βγ holds
  - **Decomposition rule**: If α → βγ holds, then α → β holds and α → γ holds
  - **Pseudotransitivity** **rule**:If α → β holds *a*nd γβ → σ holds, then αγ → σ holds

```
F+ = F
repeat
	for each functional dependency f in F+
		apply reflexivity and augmentation rules on f
		add the resulting functional dependencies to F+
	for each pair of functional dependencies f1and f2 in F+
		if f1 and f2 can be combined using transitivity
			then add the resulting functional dependency to F+
until F + does not change any further
```



### Closure of Attribute Sets

- α+: define the **closure** of α **under** ***F*** as the set of attributes that are functionally determined by a under *F*

```
result := a;
while (changes to result) do
	for each β → γ in F do
	begin
		if β ⊆ result then result := result ∪ γ
	end
```

- Testing for **superkey**: To test if a is a superkey, we compute a+, and check if a+ contains all attributes of *R*
- Testing **functional dependencies**: To check if a functional dependency α → β holds (or, in other words, is in *F*+), just check if β ⊆ α+
- Computing **closure of F**: For each γ ⊆ *R,* we find the closure γ+, and for each *S* ⊆ γ+, we output a functional dependency γ → *S*



### Extraneous Attributes

- An attribute of a functional dependency in *F* is **extraneous** if we can remove it without changing *F*+
- Removing an attribute from the **left** side of a functional dependency could make it a **stronger** constraint

![image-20211015120014946](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211015120014946.png)

![image-20211015120135482](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211015120135482.png)

- Removing an attribute from the **right** side of a functional dependency could make it a **weaker** constraint

![image-20211015120023506](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211015120023506.png)

![image-20211015120044688](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211015120044688.png)



### Canonical Cover

- all the **functional dependencies** in *F* are satisfied in the **updated** database state, if an update violates any functional dependencies in the set *F,* the system must **roll back** the update
- A **canonical cover** for *F* is a set of dependencies *Fc* such that
  - *F* logically implies **all dependencies** in *Fc* , and
  - *Fc* logically implies all dependencies in *F,* and
  - No functional dependency in *Fc* contains an **extraneous attribute**, and
  - Each **left side** of functional dependency in *Fc* is **unique**. That is, there are no two dependencies in *Fc*: α1 → β1 and α2 → β2 such that α1 = α2

```
repeat
	Use the union rule to replace any dependencies in F of the form
	/ * Note: Union rule may become applicable after some extraneous attributes have been deleted, so it has to be re-applied */
	α → β1 & α → β2 with α → β1β2
	Find a functional dependency α → β in Fc with an extraneous attribute either in α or in β
	/* Note: test for extraneous attributes done using Fc, not F */
	If an extraneous attribute is found, delete it from α → β
until  (Fc not change)
```



### Dependency Preservation

- Let *Fi* be the set of dependencies *F+* that include only attributes in *Ri.* A decomposition is **dependency preserving**, if (*F*1 ∪ *F*2 ∪ *…* ∪ *F*n)+ = *F+*
- The **restriction** of *F* to *R*i is the set *F*i of all functional dependencies in ***F+*** that include **only** **attributes** of *R*i
- Since all functional dependencies in a restriction involve attributes of **only one relation schema**, it is possible to test such a dependency for satisfaction by checking only one relation
- Note that the **definition of restriction** uses all dependencies in in ***F+***, not just those in *F*
- To check if a dependency α → β is preserved in a decomposition of *R* into *R*1, *R*2, …, *R*n , we apply the following test (with attribute closure done with respect to ***F***)

```
result = α
repeat
	for each Ri in the decomposition
		t = (result ∩ Ri)+ ∩ Ri
		result = result ∪ t
until (result does not change)
```

- If *result* contains **all attributes in β**, then the functional dependency α → β is preserved
- This procedure takes **polynomial time**, instead of the **exponential time** required to compute *F+* and (*F*1 ∪ *F*2 ∪ *…* ∪ *F*n)+



## Algorithms for Decomposition using Functional Dependencies

### Testing for BCNF

- To check if a **non-trivial** dependency α → β causes a **violation** of BCNF
  - compute a+ (the attribute closure of a), and
  - verify that it includes all attributes of *R*, that is, it is a **superkey** of *R*
- **Simplified test**: To check if a relation schema *R* is in BCNF, it suffices to check only the dependencies in the given set ***F*** for violation of BCNF, rather than checking all dependencies in *F*+
- However, simplified test using only F is **incorrect** when testing a relation in a **decomposition** of R



### Testing Decomposition for BCNF

- Either test Ri for BCNF with respect to the **restriction** of **F+** to **Ri** (that is, all FDs in F+ that contain only attributes from Ri)
- Or use the original set of dependencies ***F*** that hold on ***R***, but with the following test:
  - for every set of attributes α ⊆ *Ri*, check that **α+** either includes **no attribute of** ***Ri* - α**, or includes **all attributes of *Ri***
  - If the condition is violated by some α → β in F+, the dependency **α → (α+ - α) ∩ Ri** can be shown to hold on *Ri*, and ***Ri* violates BCNF**
  - We use above dependency to **decompose *Ri***



### BCNF Decomposition Algorithm

- each *Ri* is in BCNF, and decomposition is lossless-join

```
result := {R};
done := false;
compute F+;
while (not done) do
	if (there is a schema Ri in result that is not in BCNF)
	then begin
		let α → β be a nontrivial functional dependency that holds on Ri such that α → Ri is not in F+, and α ∩ β  = ∅;
		result := (result – Ri) ∪ (Ri – β) ∪ (α, β);
	end
else done := true; 
```



### Testing for 3NF

- There is always a **lossless-join**, **dependency-preserving** decomposition into 3NF
- Need to check only FDs in ***F***, **need not** check all FDs in ***F+***, use **attribute closure** to check for each dependency α → β, if α is a **superkey**
- If α is not a superkey, we have to verify if each attribute in β is contained in a **candidate key** of *R*, testing for 3NF has been shown to be NP-hard



### 3NF Decomposition Algorithm

- decomposition into third normal form (described shortly) can be done in **polynomial time**
- Each relation schema *Ri* is in **3NF**
- Decomposition is dependency **preserving** and **lossless-join**

```
Let Fc be a canonical cover for F;
i := 0;
for each functional dependency α → β in Fc do
	if none of the schemas Rj, 1 ≤ j ≤ i contains αβ
	then begin
		i := i + 1;
		Ri := αβ
	end
if none of the schemas Rj, 1 ≤ j ≤ i contains a candidate key for R
then begin
	i := i + 1;
	Ri := any candidate key for R;
end
```

```
/* Optionally, remove redundant relations */
repeat
if any schema Rj is contained in another schema Rk
then /* delete Rj */
	Rj = Ri;
	i = i - 1;
return (R1, R2, ..., Ri)
```



### Comparison of BCNF and 3NF

- It is always possible to decompose a relation into a set of relations that are in **3NF** such that:
  - The decomposition is **lossless**
  - The dependencies are **preserved**

- It is always possible to decompose a relation into a set of relations that are in **BCNF** such that:
  - The decomposition is **lossless**
  - It **may not be possible** to preserve dependencies.



### Design Goals

- Goal for a relational database design is:
  - **BCNF**
  - Lossless join
  - Dependency preservation

- If we cannot achieve this, we accept one of
  - Lack of **dependency preservation**
  - Redundancy due to use of **3NF**

- SQL does not provide a direct way of specifying functional dependencies other than superkeys



## Decomposition Using Multivalued Dependencies 

### Multivalued Dependencies

- Let *R* be a relation schema and let α ⊆ *R* and β ⊆ *R.* The multivalued dependency **α →→ β** holds on *R* if in any legal relation *r(R),* for **all pairs** for tuples *t*1 and *t2* in *r* such that *t*1[a] = *t2* [a], there exist tuples *t3* and *t*4 in *r* such that:
  - *t*1[α] = *t2*[α] = *t*3[α] = *t*4[α]
  - *t*3[β] = *t*1[β] & *t*4[β] = *t*2[β]
  - *t*3[*R –* β] = *t*2[*R –* β] & *t*4[*R –* β] = *t*1[*R –* β]

![image-20211020005512069](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211020005512069.png)

- Let *R* be a relation schema with a set of attributes that are partitioned into 3 nonempty subsets: *Y, Z, W*, *Y* →→ *Z* if *Y* →→ *W*



### Use of MVDs

- To test relations to determine whether they are legal under a given set of **functional and multivalued dependencies**
- To specify **constraints** on the set of legal relations. We shall concern ourselves *only* with relations that satisfy a given set of functional and multivalued dependencies
- If a relation *r* fails to satisfy a given multivalued dependency, we can construct a relations that does satisfy the multivalued dependency by adding tuples to *r*



### Theory of MVDs

- If *Y* → *Z*  then *Y* →→ *Z*
- The **closure** D+ of *D* is the set of all functional and multivalued dependencies logically implied by *D*



### Fourth Normal Form

- A relation schema *R* is in 4NF with respect to a set *D* of **functional and multivalued dependencies** if for all multivalued dependencies in ***D*+** of the form a α →→ β, where α ⊆ *R* and β ⊆ *R,* at least one of the following hold:
  - α →→ β is trivial (i.e., β ⊆ α or α ∪ β *= R)*
  - α is a superkey for schema *R*



### Restriction of MVDs

- The restriction of D to Ri is the set Di consisting of
  - All functional dependencies in **D+** that include **only** attributes of Ri
  - All multivalued dependencies of the form α →→ *(*β ∩ Ri), where α ⊆ Ri and α →→ β is in D+



### 4NF Decomposition Algorithm

- Note: each Ri is in **4NF**, and decomposition is **lossless-join**

```
result := {R};
done := false;
compute D+;
Let Di denote the restriction of D+ to Ri
while (not done)
	if (there is a schema Ri in result that is not in 4NF) then
	begin
		let α →→ β be a nontrivial multivalued dependency that holds on Ri such that α →→ Ri is not in Di, and α ∩ β = ∅;
		result := (result - Ri) ∪ (Ri - β) ∪ (α, β);
	end
	else done := true;
```



## More Normal Form

- **Join dependencies** generalize multivalued dependencies, lead to **project-join normal form (PJNF)** (also called **fifth normal form**)
- A class of even more general constraints, leads to a normal form called **domain-key normal form**
- Problem with these generalized constraints: are **hard to reason with**, and no set of sound and complete set of inference rules exists, hence rarely used



## Database-Design Process

### Overall Database Design Process

- *R* could have been generated when converting **E-R diagram** to **a set of tables**
- *R* could have been a single relation containing *all* attributes that are of interest (called **universal relation**)
- **Normalization** breaks *R* into smaller relations
- *R* could have been the result of some ad hoc design of relations, which we then test/convert to normal form



### Denormalization for Performance

- May want to use **non-normalized** schema for **performance**
- **faster** lookup, extra **space** and extra execution **time** for updates, extra coding work for programmer and possibility of error in **extra code**



### Other Design Issues

- Is an example of a **crosstab**, where values for one attribute become column names



## Modelling Temporal Data

- **Temporal data** have an association time interval during which the data are *valid*

- **Snapshot**: the value of the data at a particular point in time

- Adding a temporal component results in functional dependencies like

   *ID* → *street, city*. Not holding, because the address varies over time

- A **temporal functional dependency** X → Y holds on schema *R* if the functional dependency X → Y holds on all snapshots for all legal instances r(*R*)
- In practice, database designers may add **start and end time** attributes to relations
- **Foreign key** references may be to current version of data, or to data at a point in time



## Explanation

- 无损分解定义8：关系代数定义
- 无损分解判断方法18：在*函数依赖闭包*中满足任一函数依赖
- 函数依赖定义13
- 函数依赖闭包定义14
- **函数依赖闭包算法**42
- **属性集闭包定义和算法**43
- 多值依赖定义76
- 多值依赖闭包定义81
- 依赖保持定义20
- **依赖保持算法**57：在*函数依赖集合*中测试
- BC范式定义23：*函数依赖闭包*中每一函数依赖至少满足一个条件
- **BC范式分解算法**63：找到*函数依赖集合*违反BC范式的函数依赖
- BC范式可以保证无损分解，不能保证依赖保持
- 若函数依赖集合中没有依赖违反BC范式，则函数依赖闭包中不会有依赖违反BC范式
- 第三范式定义28：*函数依赖闭包*中每一函数依赖至少满足一个条件
- **第三范式分解算法**68：在*正则覆盖*基础上分解
- 第三范式可以保证无损分解和依赖保持，但会造成信息重复和需要使用null值
- 若函数依赖集合中没有依赖违反第三范式，则函数依赖闭包中不会有依赖违反第三范式
- 第四范式定义82：*多值依赖闭包*中每一多值依赖至少满足一个条件
- **第四范式分解算法**84：找到*多值依赖集合*违反第四范式的多值依赖
- 无关属性定义46：*函数依赖闭包*中冗余
- 正则覆盖定义52
- **正则覆盖算法**53：删去无关属性，使用并规则
