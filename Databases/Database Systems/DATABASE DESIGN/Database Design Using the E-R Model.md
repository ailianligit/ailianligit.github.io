# Database Design Using the E-R Model

## Overview of the Design Process

### Design Phases

- Initial phase: characterize fully the **data needs** of the prospective database users
- Second phase: choosing a **data model**
- Final Phase: Moving from an abstract data model to the **implementation** of the database
  - Logical Design: Deciding on the **database schema**
  - Physical Design: Deciding on the physical layout of the database      



### Design Approaches

- Entity Relationship Model
  - Models an enterprise as a collection of ***entities*** and ***relationships***
  - Entity: a “thing” or “object” in the enterprise that is distinguishable from other objects, described by **a set of *attributes***
  - Relationship: an **association** among several entities
  - Represented diagrammatically by an ***entity-relationship diagram***

- Normalization Theory: Formalize what designs are bad, and test for them



## The Entity-Relationship Model

- The ER data model employs three basic concepts: **entity sets**, **relationship sets** & **attributes**



### Entity Sets

- An **entity** is an object that exists and is distinguishable from other objects,  is a set of entities of the same type that **share the same properties**
- An entity is represented by a set of **attributes**, a subset of the attributes form a **primary key** of the entity set; i.e., uniquely identifying each member of the set

![image-20211008210822728](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008210822728.png)



### Relationship Sets

- A relationship set is a mathematical relation among *n* ≥ 2 entities, each taken from entity sets

![image-20211008210755927](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008210755927.png)

- An **attribute** can also be associated with a relationship set

![image-20211008210801709](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008210801709.png)

- Entity sets of a relationship need not be distinct. Each occurrence of an entity set plays a “role” in the relationship

![image-20211008210930632](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008210930632.png)



## Complex Attributes

- Attribute types: **Simple** attributes, **composite** attributes, **Single-valued** attributes, **multivalued** attributes & **Derived** attributes
- **Domain**: the set of permitted values for each attribute



## Mapping Cardinalities

- Express the number of entities to which another entity can be associated via a relationship set
- For a **binary** relationship set the mapping cardinality must be one of the following types: one-to-one, one-to-many, many-to-one & many-to-many
- Some elements in *A* and *B* may not be mapped to any elements in the other set

![image-20211008212200765](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008212200765.png)

![image-20211008212207393](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008212207393.png)

![image-20211008212226996](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008212226996.png)

![image-20211008212313447](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008212313447.png)

- **Total participation**: every entity in the entity set participates in at least one relationship in the relationship set

![image-20211008212411757](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008212411757.png)

- **Partial participation**: some entities may not participate in any relationship in the relationship set
- We allow at most **one** arrow out of a **ternary** (or greater degree) relationship to indicate a cardinality constraint



## Primary Key

- Primary keys provide a way to specify how **entities and relations** are **distinguished**



### Primary key for Entity Sets

- From database perspective, the differences among entities must be expressed in terms of their **attributes**
- The **values** of the attribute values of an entity must be such that they can uniquely identify the entity
- A key for an entity is a set of attributes that suffice to **distinguish** entities from each other



### Primary Key for Relationship Sets

- To distinguish among the various relationships of a relationship set we use the individual **primary keys of the entities** in the relationship set
- If the relationship set *R* has **attributes** associated with it, then the primary key of *R* also includes the attributes
- The choice of the primary key for a relationship set depends on the **mapping cardinality** of the relationship set
  - Many-to-Many: the union of the primary keys
  - One-to-Many & Many-to-one: the primary key of the “Many” side
  - One-to-one: The primary key of either one of the participating entity sets



### Weak Entity Sets

- instead of associating a primary key with a weak entity, we use the **primary key** **of the identifying entity**, along with extra attributes, called **discriminator attributes** to uniquely identify a **weak entity**

- Every **weak entity** must be associated with an **identifying entity**; that is, the weak entity set is said to be **existence dependent** on the identifying entity set. The identifying entity set is said to **own** the weak entity set that it identifies

- The relationship associating the weak entity set with the identifying entity set is called the **identifying relationship**

- The identifying relationship is **many-to-one** from the weak entity set to the identifying entity set, and the participation of the weak entity set in the relationship is total

- The identifying relationship set should not have any **descriptive** attributes, since any such attributes can instead be associated with the **weak entity set**

- A weak entity set can participate in relationships other than the identifying relationship. A weak entity set may participate as owner in an identifying relationship with another weak entity set

- It is also possible to have a weak entity set with more than one identifying entity set. A particular weak entity would then be identified by a **combination** of entities, one from each identifying entity set

- The primary key of the weak entity set would consist of the **union of the primary keys of the identifying entity sets**, plus the **discriminator** of the weak entity set

  

![image-20211008215609689](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211008215609689.png)



## Reducing ER Diagrams to Relational Schemas

### Representing Entity Sets

- A strong entity set reduces to a schema with the same attributes
- A weak entity set becomes a table that includes a column for the primary key of the **identifying** strong entity set



### Representation of Entity Sets with Composite Attributes

- Composite attributes are **flattened out** by creating a separate attribute for each component attribute

```sql
instructor(ID,
           first_name, middle_initial, last_name,street_number, street_name, apt_number, 			city, state, zip_code, date_of_birth)
```

![image-20211009003953358](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009003953358.png)



### Representation of Entity Sets with Multivalued Attributes

- Schema *EM* has attributes corresponding to the primary key of *E* and an attribute corresponding to multivalued attribute *M*
- Each value of the multivalued attribute maps to a **separate tuple** of the relation on schema *EM*



### Representing Relationship Sets

- A **many-to-many** relationship set is represented as a schema with attributes for the primary keys of the **two** participating entity sets, and any **descriptive attributes** of the relationship set



### Redundancy & Combination of Schemas

- the schema for the **relationship set** linking a weak entity set to its corresponding strong entity set is **redundant** and does not need to be present in a relational database design based upon an E-R diagram  
- combine the schemas A and AB to form a single schema consisting of the union of attributes of both schemas
- The **primary key** of the combined schema is the primary key of the **entity set** into whose schema the relationship set schema was merged  
- Many-to-one and one-to-many relationship sets that are total on the many-side can be represented by **adding an extra attribute** to the “many” side, containing the primary key of the “one” side
- For one-to-one relationship sets, **either side** can be chosen to act as the “many” side
- If participation is ***partial*** on the “many” side, replacing a schema by an extra attribute in the schema corresponding to the “many” side could result in **null values**
- **drop** the constraint referencing the entity set into whose schema the **relationship set** **schema** is merged, and **add** the other foreign-key constraints to the **combined schema**



## Extended E-R Features

### Specialization

- Top-down design process; we designate **sub-groupings** within an entity set that are **distinctive** from other entities in the set. These sub-groupings become **lower-level entity sets** that have attributes or participate in relationships that **do not apply** to the higher-level entity set
- Depicted by a *triangle* component labeled ISA (e.g., *instructor* “is a” *person*)
- **Attribute inheritance**: a lower-level entity set inherits all the **attributes** and **relationship** participation of the higher-level entity set to which it is linked

![image-20211009165230525](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009165230525.png)



### Representing Specialization via Schemas

- Method 1: 
  - Form a schema for the higher-level entity
  - Form a schema for each lower-level entity set, include **primary key of higher-level** entity set and **local** attributes
  - Drawback: getting information about, an *employee* requires accessing two relations, the one corresponding to the **low-level** schema and the one corresponding to the **high-level** schema

![image-20211009165857679](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009165857679.png)

- Method 2:
  - Form a schema for each entity set with **all** local and inherited attributes
  - Drawback: *name, street* and *city* may be stored redundantly for people who are both students and employees

![image-20211009165957441](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009165957441.png)



### Generalization

- **A bottom-up design process** – combine a number of entity sets that share the same features into a higher-level entity set



### Completeness constraint

- **Completeness constraint**: specifies whether or not an entity in the higher-level entity set must belong to at least one of the **lower-level** entity sets within a generalization (**total** & **partial**)
- **Partial** generalization is the **default**
- We can specify total generalization in an ER diagram by adding the keyword **total** in the diagram and drawing a **dashed line** from the keyword to the corresponding **hollow arrow-head** to which it applies (for a total generalization), or to the set of hollow arrow-heads to which it applies (for an overlapping generalization)
- the completeness constraint for a generalized higher-level entity set is usually **total**



### Aggregation

- Aggregation is an abstraction through which **relationships** are treated as **higherlevel** **entities**
- To represent aggregation, create a schema containing **primary key of the aggregated relationship**, the **primary key of the associated entity set** & any **descriptive attributes**

![image-20211009171146863](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009171146863.png)

![image-20211009171154297](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009171154297.png)



## Entity-Relationship Design Issues

![image-20211009172139844](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009172139844.png)

![image-20211009172146407](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009172146407.png)

- Possible guideline is to designate a relationship set to describe an **action** that occurs between entities
- Any non-binary relationship can be represented using binary relationships by creating an **artificial** entity set

![image-20211009173330631](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009173330631.png)

- There may be instances in the translated schema that **cannot** correspond to any instance of *R*, we can avoid creating an identifying attribute by **making E a weak entity set** (described shortly) identified by the three relationship sets



### E-R Design Decisions

- The use of an **attribute** or **entity set** to represent an object
- Whether a real-world concept is best expressed by an **entity set** or a **relationship set**
- The use of a **ternary** relationship versus **a pair of binary** relationships
- The use of a **strong** or **weak** entity set
- The use of **specialization/generalization**: contributes to modularity in the design
- The use of **aggregation** – can treat the aggregate entity set as a single unit without concern for the details of its internal structure



## Alternative Notations for Modeling Data

![image-20211009164728347](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009164728347.png)

![image-20211009164750375](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009164750375.png)

![image-20211009164801235](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20211009164801235.png)
