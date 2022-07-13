# SQL
Is a standard language for storing, manipulating and retrieving data in relational databases

SQL implementations vary between R[[DBMS]], the concepts are the same but the implementations vary a little bit.

4 types of languages:
 - **Data Query Language (DQL)** used to query for data
 - **Data Definition Language (DDL)** used for defining schemas
 - **Data Control Language (DCL)** user & permissions management
 - **Data Manipulation Language (DML)** manipulating data



### Primary keys
`PRIMARY KEY` is a constraint that uniquely identifies each row in a table

**Surrogate key** is a key that has no mapping in the real world
**Natural key** has a mapping in the real world (using EMSO as a primary key)

#### Composite keys
A composite key is made by the combination of two or more columns in a table. 

### Foreign keys
`FOREIGN KEY` is a key that refers to the `PRIMARY KEY` in another table
Is used to prevent actions taht would destroy links between tables.

There can be multiple **foreign keys**, and they can link back to the same table

### Atributes
 - default 'default name'
 - not null
 - auto incrememnt

## Data types
 - nvarchar for unicode

### Converting data types
 - cast(**attribute** as integer) -> **attribute**::int (cast operator)
 - select 17 || '76' -> implicit casting (coercion)



### Create

```SQL
CREATE TABLE student (
	student_id INT PRIMARY KEY,
	name VARCHAR(20),
	major VARCHAR(20)
);
```

### Insert

```SQL
INSERT INTO student VALUES(2, "evan", "CS");

INSERT INTO student(student_id, name) VALUES(2, "evan"));


insert into [tableName] ([key], [name]) query;
```


## Update
```sql
update [tableName] set [name] = [name] + 'something';

-- can be used with a join..

update [tableName] set [name] = [tableName1] + something
from [tableName] join [tableName1]; 

```


## Functions
**COUNT**
```SQL
SELECT COUNT(super_id) from employee; --Counts non NULL values
```
AVG, SUM




### Union

```SQL
SELECT name from students
UNION
SELECT name from profesorji
```



### Joins

**INNER JOIN**
```SQL
SELECT student.id, student.name, mentor.ime
FROM student
join mentor
on student.mentorId = mentor.id;
```


**LEFT JOIN**
Vrne vse student in se kjer mentor.id == student.mentorId
```SQL
SELECT student.id, student.name, mentor.ime
FROM student
join mentor
on student.mentorId = mentor.id;
```

![[Pasted image 20220713134930.png]]


## Window function
`OVER()` is kinda like a subquery

**function** ( [ arguments ] ) OVER ( [ PARTITION BY **expr1** ]    [ ORDER BY **expr2** ] )

`order by` - orders rows within the window. Some window functions use it as an implicit cumulative window clause.

```SQL
SELECT e.\*, max(salary) over(partition by dept_name) as max_salary
from employee e;
```
So this will create an additional collumn where we will display the max salary for every dept_name

`PARTITION` creates sub groups kinda like a group by

### ROW_NUMBER
Just adds a new collumn n of row 

```SQL
SELECT e.\*, row_number() over(partitiion by dept_name order by emp_id) as rn
from employee e;
```
So this will add a column where we will count for every dept_name, and it will be order by employee id. So like subtables in a table.

If you want to display the first 2 employees from each department that joined the company (assuming id is incrementing for every employee)

```SQL
SELECT \* FROM (
	SELECT e.\*, row_number() over(partitiion by dept_name order by emp_id) as rn
	from employee e;
) x where x.rn < 3;
```

### RANK
Similar to ROW_NUMBER but it gives number depending on an atribute so the highest salaey will have the highest rank. Same salary will have the same rank but the next salary will have +2 rank (It still counts). 
```SQL
SELECT e.\*, rank() over(partitiion by dept_name order by emp_id desc) as rn
from employee e;
```

### DENSE_RANK
Dense rank it the same as rank but it will not skip a value when having duplicate salary.

### Lag
-- To be added -- 
### Lead
-- To be added -- 

### Subqueries
Similar to window functions (performance can differ)
```SQL

--Subqueries
select EmployeeId, salary, (Select AVG(salary) from Employee) as AllAvgSalary from Employee;

-- Window, Over method
select EmployeeId, salary, AVG(salary) over () as AllAvgSalary from Employee;
```



## Cohort analysis
 - a cohort is a group with a shared characteristic
 - is a subset of behavioural analytics, we brake it into related groups for analysis

## View
Just a virtual table

create view [viewName] as (subquery)
drop view [viewName]


ntile() 
 -  a ranking function that distributes the rows in an ordered partition into a specified number of groups
 - NTILE(4) the number of groups so now we are looking at the spread of quartiles 
 - so basically it puts rows in groups of quartiles 1-4


## Strings
actully varchar

left(string, length)
 - left('ABCDEF', 3) -> 'ABC'
 
charindex(char, string [, start_pos]) -> the index of first occurance of char
 - can be used with left for example splitting words by searching for the first ' '
 - left('Name Surname', charindex(' ', 'Name Surname'))

char_length

upper/lower 

replace(string, pattern, [, replacement]
 - removes all occurrences of patern
 - if replacement is ommited then it just removes

CONCAT( **expr1** [ ,  **exprN** ... ] )
 - || -> alternative syntax

### Wildcards
A wildcard char is used to substitute one or more chars.

(%) - zero, one or multiple chars
(\_) - represents one single char
([]) - represents any single chars in the bracket h[oa]t = hot, hat
^ - represents any single chars not in the brackets h[\^oa]t
\- - range a[b-d] = ab, ac, ad

**LIKE**
(%) - zero, one or multiple chars
(_) - represents one single char





## Terminal Commands
- show databases
- use [db]
- desc [tableName]


## SELECT

[ WITH ... ] (defining CTEs)
SELECT
   [ TOP n ]
   ...
[ INTO ... ]
[ FROM ...
   [ AT | BEFORE ... ]
   [ CHANGES ... ]
   [ CONNECT BY ... ]
   [ JOIN ... ]
   [ MATCH_RECOGNIZE ... ]
   [ PIVOT | UNPIVOT ... ]
   [ VALUES ... ]
   [ SAMPLE ... ] ]
[ WHERE ... ]
[ GROUP BY ...
   [ HAVING ... ] ]
[ QUALIFY ... ]
[ ORDER BY ... ]
[ LIMIT ... ]


#### pivot
 - rotates a table by turning the unique values from one column and aggregating 
 - change column names to months in a row and summing something
 - (empid, month, sales) into a wider table (e.g. empid, jan_sales, feb_sales, mar_sales)
 
 ```sql
 select * from (
	 select month(orderdate) as month, count(*) as orders from
	 sales.orders group by month(orderdate))
	 pivot(sum(orders) for month in (1, 2, 3));
```
	
change the column names
```sql
select * from (
    select month(orderdate) as month, count(*) as orders from
    sales.orders group by month(orderdate))
    pivot(sum(orders) for month in (1, 2, 3)) as p ('jan', 'feb', 'mar');
```
 
	
#### unpivot
 - rotates a table by transforming columns into rows
 - not exactly the opposite of unpivot as it doesn't undo the aggregation
 - (e.g. empid, jan_sales, feb_sales, mar_sales) to (empid, month, sales)

```sql
select * from sales
    unpivot(sales for month in (jan, feb, mar, april))
    order by empid;
```
 

#### WHERE
- filters the result of the from clause in the select statement
- used before the group by


#### group by rollup
 - produces sub-total rows in addition to the grouped rows.
 - sub-total rows are just further aggregated values from grouped rows
 - kinda similar to partition by
```sql
select state, city, sum(price) as profit 
 from products as p, sales as s
 where s.product_id = p.product_id
 group by rollup (state, city)
 order by state, city nulls last;
-- but doing like this the last row for each state will have a null city attribute, and the last row will have a null null for total sales
 
```
 
 
 
 
#### having
 - filters rows produced by `group by`
 - can contain aggregate functions
 - used after the group by
 
#### qualify
 - filters results of window functions
 - otherwise filtering requires nesting

```sql
-- nested
select * 
from (
	 select i, p, o, 
			row_number() over (partition by p order by o) as row_num
		from qt
	)
where row_num = 1
;

-- with qualify
select i, p, o
from qt
qualify row_number() over (partition by p order by o) = 1
;
```
 
 
 

#### order of execution
1. From and Joins : since these two forms the basis of the query
2. Where : Filters out the rows
3. Group By : Grouping values based on the column specified in the Group By clause
4. Having : Filters out the grouped rows
5. Select, window
6. Distinct : Rows with duplicate values in the column marked as Distinct are discarded
7. Order By : Rows are sorted based on Order By clause
8. Limit, Offset : Finally the limit or offset is applied


## Case

```sql
SELECT companyname, sum(CASE WHEN diff < 11 THEN 1 ELSE 0 END) as "do_10",
                    sum(CASE WHEN between 10 and 50 THEN 1 ELSE 0 END) as "11-50",
                    sum(CASE WHEN between 50 and 100 THEN 1 ELSE 0 END) as "51-100",
                    sum(CASE WHEN 100 < diff THEN 1 ELSE 0 END) as "vec"
```


### NVL
- just an alias for ifnull
- if  **expr1** is NULL, return **expr2** otherwise return **expr1**