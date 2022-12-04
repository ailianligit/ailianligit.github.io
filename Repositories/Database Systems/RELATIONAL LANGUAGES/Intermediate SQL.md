# Intermediate SQL

## Join Expressions

### Natural Join

- Natural join matches tuples with the **same** **values** for all **common** **attributes**, and retains **only one copy** of each common column

```sql
select A1, A2, … An
from r1 natural join r2 natural join .. natural join rn
where P;
```

- Dangerous: query may omit tuples where the student takes a course in a department other than the student's own department

```sql
#correct version
select *
from student natural join takes, course
where takes.course_id = course.course_id;

#incorrect version
select name, title
from student natural join takes natural join course;
```

- To avoid the danger of equating attributes erroneously, we can use the “**using**” construct that allows us to specify exactly which columns should be equated
- Join Condition: This predicate is written like a **where** clause predicate except for the use of the keyword **on**



### Outer Join

- An extension of the join operation that avoids loss of information
- Computes the join and then adds tuples from one relation that does not match tuples in the other relation to the result of the join, using ***null*** values
- full outer join = nature join + left outer join + right outer join



### Joined Types and Conditions

- Join operations take two relations and return as a result another **relation**
- **Join condition**: defines which tuples in the two relations match
- **Join type**: defines how tuples in each relation that do not match any tuple in the other relation (based on the join condition) are treated

![image-20210922174655066](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210922174655066.png)



## Views

### View Definition

- Any relation that is not of the conceptual model but is made visible to a user as a “virtual relation” is called a view
- View definition is not the same as creating a new relation by evaluating the query expression. Rather, a view definition causes the **saving of an expression**; the expression is substituted into queries using the view



### Materialized Views

- **Physical** copy created when the view is defined
- If relations used in the query are updated, the materialized view result becomes **out of date**, need to **maintain** the view, by updating the view whenever the underlying relations are updated



### View Updates in SQL

- The **from** clause has **only one database relation**
- The **select** clause contains **only attribute names** of the relation, and does **not** have any expressions, aggregates, or distinct specification
- Any attribute not listed in the **select** clause **can be set to null**
- The query does **not** have a **group by** or **having** clause



## Transactions

- A **transaction** consists of a sequence of query and/or update statements and is a “**unit**” of work
- The SQL standard specifies that a transaction begins implicitly when an SQL statement is executed
- The transaction must end with one of the following statements:
  - **Commit work**. The updates performed by the transaction become **permanent** in the database
  - **Rollback work**. **All** the updates performed by the SQL statements in the transaction are **undone**

- **Atomic transaction**: either fully executed or rolled back as if it never occurred
- **Isolation** from **concurrent** transactions



## Integrity Constraints

- §Integrity constraints guard against accidental damage to the database, by ensuring that authorized changes to the database do not result in a loss of **data consistency**



### Constraints on a Single Relation

- not null
- primary key
- unique: The unique specification states that the attributes *A*1, *A*2, …, *A*m  form a **candidate key**, candidate keys are **permitted to be null** (in contrast to primary keys)
- check(P): specifies a predicate P that must be satisfied by every tuple in a relation



### Referential Integrity

- Ensures that a value that appears in one relation for a given set of attributes also appears for a certain set of attributes in another relation
- Let A be a set of attributes. Let R and S be two relations that contain attributes A and where A is the **primary key** of S. A is said to be a **foreign key** **of R** if for any values of A appearing in R these values also appear in S
- By default, a foreign key references the **primary**-key attributes of the referenced table

```sql
#as part of the SQL create table statement
foreign key (dept_name) references department
foreign key (dept_name) references department (dept_name)
```



### Cascading Actions in Referential Integrity

```sql
create table course (
    (…
     dept_name varchar(20),
     foreign key (dept_name) references department
     on delete cascade
     on update cascade,
     ...) 
```

- Instead of cascade we can use : **set null** or **set default**



### Complex Check Conditions

```sql
check (time_slot_id  in (select time_slot_id from time_slot))
```

- The check condition states that the time_slot_id in each tuple in the *section* relation is actually the **identifier** of a time slot in the *time_slot* relation
- The condition has to be checked not only when a tuple is **inserted or modified in *section*** , but also when the relation ***time_slot* changes**



### Assertions

- An **assertion** is a predicate expressing a condition that we wish the database always to satisfy

```sql
create assertion <assertion-name> check (<predicate>);
```



## SQL Data Types and Schemas

- Built-in Data Types in SQL: date, time, timestamp, interval
- Large-Object Types: blob(binary large object), clob(character large object)
- When a query returns a large object, a **pointer** is returned rather than the large object itself
- User-Defined Types: **create type**

```sql
create type Dollars as numeric (12,2) final
```

- Types and domains are similar. Domains can have **constraints**, such as **not null**, specified on them

```sql
create domain person_name char(20) not null
```

```sql
create domain degree_level varchar(10)
constraint degree_level_test
check (value in ('Bachelors', 'Masters', 'Doctorate'));
```



## Index Definition in SQL

- An **index** on an attribute of a relation is a data structure that allows the database system to find those tuples in the relation that **have a specified value** for that attribute efficiently, without scanning through all the tuples of the relation

```sql
create index <name> on <relation-name> (attribute);
```



## Authorization

### Authorization Specification in SQL

```sql
grant <privilege list> on <relation or view> to <user list>
```

- **privilege**: select, insert, update, delete, all privileges
- Granting a privilege on a **view** does not imply granting any privileges on the **underlying** relations
- **user list** is a user-id, *public* or a role



### Revoking Authorization in SQL

```sql
revoke <privilege list> on <relation or view> from <user list>
```

- privilege-list may be **all** to revoke all privileges the revokee may hold
- If revokee-list includes **public,** all users lose the privilege except those granted it explicitly
- If the same privilege was granted twice to the same user by **different** grantees, the user may retain the privilege after the revocation
- All privileges that **depend on** the privilege being revoked are also revoked



### Roles

```sql
create a role <name>
grant <role> to <users>
#role2 inherits all privileges of role1
grant <role1> to <role2>
```



### Other Authorization Features

- **references** privilege to **create foreign key**

```sql
grant reference (dept_name) on department to Mariano
```

- **transfer** of privileges

```sql
grant select on department to Amit with grant option;
revoke select on department from Amit, Satoshi cascade;
revoke select on department from Amit, Satoshi restrict;
```



## Exercises

```sql
#Question 1
#use an outer join
select employee_name
from employee natural left outer join manages
where manager_name = null

#use no outer join at all
select A.employee_name
from employee as A, manages as B
where A.employee_name = B.employee_name
	and manager_name = null
	
#Question 3
create view tot credits(year, tot credits) as (select year, sum(credits)
                                               from takes natural join course
                                               group by year)
```