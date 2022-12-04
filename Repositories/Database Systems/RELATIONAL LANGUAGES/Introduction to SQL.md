# Introduction to SQL

## SQL Data Definition

### Create Table Construct

```sql
create table r 
	(A1 D1, A2 D2, ..., An Dn,
	(integrity-constraint1),
	...,
	(integrity-constraintk))
```

- each *Ai* is an **attribute** name in the schema of relation *r*
- *Di* is the **data type** of values in the domain of attribute *Ai*
- Types of integrity constraints: **primary key** (*A*1, ..., *An* ), **foreign key** (*A*m, ..., *An* ) **references** *r* & **not null**
- SQL prevents any update to the database that violates an integrity constraint

```sql
create table instructor (
	ID			char(5),
	name		varchar(20) not null,
	dept_name  	varchar(20),
	salary		numeric(8,2),
	primary key (ID),
	foreign key (dept_name) references department);
```



### Updates to Tables

```sql
insert into instructor values ('10211', 'Smith', 'Biology', 66000);
delete from r
drop table r
alter table r add A D
alter table r drop A
```



## Basic Query Structure of SQL Queries

### Basic Query Structure

```sql
select A1, A2, ..., An
from r1, r2, ..., rm
where P
```

- *P* is a **predicate**
- The result of an SQL query is a **relation**



### The select Clause

- The select clause lists the attributes desired in the result of a query, corresponds to the **projection** operation of the relational algebra
- SQL names are case **insensitive**
- SQL allows **duplicates** in relations as well as in query results. To force the elimination of duplicates, insert the keyword **distinct** after select. The keyword **all** specifies that duplicates should not be removed
- An **asterisk** in the select clause denotes “all attributes”

```sql
select * from instructor
```

- An attribute can be a **literal with no from** clause `select '437'`. Results is a table with one column and a single row with value “437”. Can give the **column a name** using `select '437' as FOO`

- An attribute can be a **literal with from** clause. Result is a table with one column and ***N* rows** (total number of tuples in the 'from' table), each row with value “A”
- The select clause can contain **arithmetic** expressions



### The where Clause

- The where clause specifies conditions that the result must satisfy. Corresponds to the **selection predicate** of the relational algebra
- **Logical connectives**, **comparison operators** & **arithmetic expressions** are permitted



### The from Clause

- The from clause lists the relations involved in the query. Corresponds to the **Cartesian product** operation of the relational algebra

- generates every possible instructor with all attributes from both relations
- For common attributes, the attributes in the resulting table are renamed using the relation name



## Additional Basic Operations

### The Rename Operation

- The SQL allows renaming relations and attributes using the as clause: `old-name as new-name`
- Keyword as is optional and may be **omitted** `instructor as T` ≡ `instructor T`

```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept_name = 'Comp. Sci.’
```



### String Operations

- The operator **like** uses patterns that are described using two special characters: percent (%). The % character matches any **substring**; underscore (_). The _ character matches any **character**
- use backslash (\\) as the **escape character** `like '100 \%'  escape '\'`
- Patterns are **case sensitive**
- 'Intro%' matches any string **beginning** with “Intro”
- '%Comp%' matches any string containing “Comp” as a **substring**
- '_ _ _' matches any string of exactly three characters
- '_ _ _ %' matches any string of at least three characters



### Ordering the Display of Tuples

- List in **alphabetic** order the names of all instructors `order by` 
- We may specify **desc** for descending order or **asc** for ascending order, for each attribute; **ascending** order is the default



### Where Clause Predicates

```sql
select name
from instructor
where salary between 90000 and 100000
```

```sql
select name, course_id
from instructor, teaches
where (instructor.ID, dept_name) = (teaches.ID, 'Biology');
```



## Set Operations

- Set operations **union**, **intersect**, and **except**
- Each of the operations automatically **eliminates duplicates**



## Null Values

- null signifies an **unknown** value or that a value does **not exist**
- The result of any **arithmetic** expression involving null is **null**
- The predicate **is null** can be used to check for null values, the predicate **is not null** succeeds if the value on which it is applied is not null
- SQL treats as **unknown** the result of any **comparison** involving a null value
- Result of **where** clause predicate is treated as ***false*** if it evaluates to ***unknown***



## Aggregate Functions

- avg, min, max, sum, count

```sql
select count (distinct ID)
from teaches
where semester = 'Spring' and year = 2018;
```

- Attributes in select clause outside of aggregate functions must appear in **group by** list

```sql
select dept_name, avg (salary) as avg_salary
from instructor
group by dept_name;
```

- predicates in the **having** clause are applied **after** the formation of groups whereas predicates in the **where** clause are applied **before** forming groups

```sql
select dept_name, avg (salary) as avg_salary
from instructor
group by dept_name
having avg (salary) > 42000;
```



## Nested Subqueries

```sql
select A1, A2, ..., An
from r1, r2, ..., rm
where P
```

- **From clause:** *ri* can be replaced by any valid subquery
- **Where clause:** *P* can be replaced with an expression of the form: `B <operation> (subquery)`
- **Select clause:** *Ai*  can be replaced be a subquery that generates a single value



### Set Membership

```sql
select count (distinct ID)
from takes
where (course_id, sec_id, semester, year) in
					(select course_id, sec_id, semester, year
					from teaches
					where teaches.ID= 10101);
```



### Set Comparison

```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept_name = 'Comp. Sci.’
```

```sql
select name
from instructor
where salary > some (select salary
					from instructor
					where dept name = 'Comp. Sci.');
```

- (= **some**) 恒等于 **in**

  However, ( ≠ **some**) 不恒等于 **not in**

- ( ≠ **all**) 恒等于 **not in**

  However, (= **all**) 不恒等于 **in**

- The **exists** construct returns the value true if the argument subquery is nonempty



### Test for Empty Relations

```sql
(select course_id  from section where sem = 'Fall' and year = 2017)
intersect
(select course_id  from section where sem = 'Spring' and year = 2018)
```

```sql
select distinct course_id
from section
where semester = 'Fall' and year= 2017 and
		course_id in (select course_id
			from section
			where semester = 'Spring' and year= 2018);
```

```sql
select course_id
from section as S
where semester = 'Fall' and year = 2017 and
		exists  (select *
			from section as T
			where semester = 'Spring' and year= 2018
				and S.course_id = T.course_id);
```

- "X belongs to Y" equals to `where not exists (A except B)`



### Test for Absence of Duplicate Tuples

- The **unique** construct evaluates to “true” if a given subquery contains no duplicates



### Subqueries in the From Clause

```sql
select dept_name, avg (salary) as avg_salary
from instructor
group by dept_name
having avg (salary) > 42000;
```

```sql
select dept_name, avg_salary
from (select dept_name, avg (salary) as avg_salary
		from instructor
		group by dept_name)
where avg_salary > 42000;
```

```sql
select dept_name, avg_salary
from (select dept_name, avg (salary)
		from instructor
		group by dept_name)
		as dept_avg (dept_name, avg_salary)
where avg_salary > 42000;
```



### The With Clause

- The with clause provides a way of defining a temporary relation whose definition is available only to the query **in which the with clause occurs**

```sql
with dept _total (dept_name, value) as
	(select dept_name, sum(salary)
	from instructor
	group by dept_name),
dept_total_avg(value) as
	(select avg(value)
	from dept_total)
select dept_name
from dept_total, dept_total_avg
where dept_total.value > dept_total_avg.value;
```



### Scalar Subquery

```sql
select dept_name,
		(select count(*)
		from instructor
		where department.dept_name = instructor.dept_name)
		as num_instructors
from department;
```



## Modification of the Database

### Deletion

```sql
delete from instructor
where dept name in (select dept name
					from department
					where building = 'Watson');
```



### Insertion

```sql
insert into instructor
	select ID, name, dept_name, 18000
	from student
	where dept_name = 'Music' and total_cred > 144;
```



### Updates

```sql
update instructor
	set salary = salary * 1.03
	where salary > 100000;
update instructor
	set salary = salary * 1.05
	where salary <= 100000;
```

```sql
update instructor
	set salary = case
				when salary <= 100000 then salary * 1.05
				else salary * 1.03
				end
```



## Exercises

```sql
#Question 1: a
select count(distinct *)
from accident
where exists(select *
             from participated, person
             where accident.report number = participated.report number
             and participated.driver id = person.driver id
             and name = 'John Smith')
             
#b
update participated
set damage amount = 3000
where report number = 'AR2197' and exists
	(select *
    from owns
    where license = 'AABB2000' and participated.driver id = owns.driver id)
```

```sql
#Question 2: a
select (distinct employee name)
from works
where company name = 'First Bank Corporation'

#b
select (distinct A.employee name)
from employee as A
where exists (select *
             from works as B, company as C
             where A.employee name = B.employee name
             and B.company name = C.company name
             and A.city = C.city)
             
#c
select (distinct A.employee name)
from employee as A
where exists (select *
             from manages as B, employee as C
             where A.employee name = B.employee name
             and B.manager name = C.employee name
             and A.city = C.city
             and A.street = C.street)
             
#d
select employee name
from works as A
where salary > (select avg(salary)
               from works B
               where A.company name = B.company_name)

#e
select company name, min(total_salary)
from(select company name, sum(salary) as total_salary
     from works
     group by company name)
```

```sql
#Question 3: a
update works
set salary = salary * 1.1
where company name = 'First Bank Corporation'

#b
update works as T
set salary = salary * 1.1
where company name = 'First Bank Corporation' and
	exists(select *
           from managers as S
           where T.employee name = S.manager name)
            
#c
delete from works
where company name = 'Small Bank Corporation'
```

```sql
#Question 4: a
select distinct A
from r

#b
select *
from r
where B = 17

#c
select distinct *
from r, s

#d
select distinct A, F
from r, s
where C = D
```

```sql
#Question 5: a
select * from r1
union
select * from r2

#b
select *
from r1
where (A, B, C) in (select *
                   from r2)

#c
select *
from r1
where (A, B, C) not in (select *
                        from r2)

#d
select distinct A, r1.B, C
from r1, r2
where r1.B = r2.B
```
