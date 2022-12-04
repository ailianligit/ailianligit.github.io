# Introduction to Relational Model

## Structure of Relational Databases

- **attributes**: *A*1, *A*2, …, *An*
- **relation schema**: *R* = (*A*1, *A*2, …, *An*)
- **relation instance**: *r*(*R*)
- **tuple** **(an element of relation)**: *t*
- **domain**: The set of allowed values for each attribute
- Attribute values are (normally) required to be **atomic**



## Database Schema

- **Database schema**: is the logical structure of the database
- **Database instance**: is a snapshot of the data in the database at a given instant in time



## Keys

- **superkey**: values for *K* are sufficient to **identify** **a unique tuple** of each possible relation *r(R)*
- **candidate key**: **minimal** superkey
- **primary key**: one of the candidate keys
- **Foreign key**: value in one relation must appear in another



## Schema Diagrams

- **Primary-key** attributes are shown **underlined**
- **Foreign-key** constraints appear as **arrows** from the foreign-key attributes of the referencing relation to the primary key of the referenced relation
- **Referential Integrity Constraint**: **two-headed arrow**

![image-20211008002759086](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008002759086.png)



## The Relational Algebra

- Not Turing-machine equivalent
- **Relational Algebra**: A procedural language consisting of a set of operations that take one or two **relations** as input and produce a new **relation** as their result
- Basic Operators: select, project, union, set difference, cartesian product & rename

- **Select Operation**: selects tuples that satisfy a given predicate
- **Project Operation**: A unary operation that returns its argument relation, with certain attributes left out (**Duplicate** rows **removed** from result, since relations are sets)
- **Cartesian-Product Operation**: combine information from any two relations
- **Join Operation**: combine a select operation and a Cartesian-Product operation into a single operation
- **The Assignment Operation**: assigning parts of it to temporary relation variables
- **The Rename Operation**



### Set Operation

- *r,* *s* must have the *same* **arity** (same number of attributes)
- The attribute domains must be **compatible**

- **Union Operation**: combine two relations 
- **Set-Intersection Operation**: to find tuples that are in both the input relations
- **Set-Difference Operation**: find tuples that are in one relation but are not in another

