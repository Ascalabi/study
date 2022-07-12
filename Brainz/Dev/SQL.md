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
```



## Functions
**COUNT**
```SQL
SELECT COUNT(super_id) from employee; --Counts non NULL values
```
AVG, SUM

### Wildcards
A wildcard char is used to substitute one or more chars.

(%) - zero, one or multiple chars
(\_) - represents one single char
([]) - represents any single chars in the bracket h[oa]t = hot, hat
^ - represents any single chars not in the btackets h[\^oa]t
\- - range a[b-d] = ab, ac, ad

**LIKE**
(%) - zero, one or multiple chars
(_) - represents one single char


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
![[Pasted image 20220630114153.png]]


## Window function
`OVER()` is kinda like a subquerie

```SQL
SELECT e.\*, max(salary) over(partitiion by dept_name) as max_salary
from employee e;
```
So this will create an additional collumn where we will display the max salary for every dept_name

`PARTITION` is kinda like group by

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
Actually the same as windows lmao (performance can differ but who cares)
```SQL

--Subqueries
select EmployeeId, salary, (Select AVG(salary) from Employee) as AllAvgSalary from Employee;

-- Window, Over method
select EmployeeId, salary, AVG(salary) over () as AllAvgSalary from Employee;
```

**Subquery in from**

**Subquery in where**
Can replace join..




## Temp tables



## Code

```SQL
--GLave dokumentov za vsako narocilo
SELECT * from SALES.ORDERS;
--Vrstice za vsako narocilo preko ORDERID
SELECT * from SALES.ORDERDETAILS;
--Vse stranke
SELECT * FROM SALES.CUSTOMERS;
--Podjetja katerim se dobavi nek prodajni nalog
SELECT * from SALES.SHIPPERS;

--Vsi produkti
SELECT * from PRODUCTION.PRODUCTS;
--Kategorija produktov
SELECT * from PRODUCTION.CATEGORIES;
--Vsi dobavitelji produkta
SELECT * from PRODUCTION.SUPPLIERS;
--Prodajniki v poodjetju EMPID iz order
SELECT * from HR.EMPLOYEES2;


-- 1. NALOGA
-- a) Izpis bruto prodaje za leto 2015 za vse dobavitelje (production.suppliers > companyname)--
SELECT distinct s.COMPANYNAME, sum(o.UNITPRICE * o.QTY * (1 - o.DISCOUNT)) over(partition by COMPANYNAME) as gross_sales FROM SALES.ORDERDETAILS o 
  join PRODUCTION.PRODUCTS p
  on o.PRODUCTID = p.PRODUCTID
  join PRODUCTION.SUPPLIERS s
  on p.SUPPLIERID = s.SUPPLIERID
  where o.ORDERID IN (SELECT ORDERID FROM SALES.ORDERS where YEAR(ORDERDATE) = 2015);



-- b) Razširi izpis z dodatnim stolpcem, ki predstavlja spremembo prodaje glede na leto 2014
select COMPANYNAME, GROSS_SALES, (GROSS_SALES - GROSS_SALES2) as abs_change 
from (SELECT distinct s.COMPANYNAME, sum(o.UNITPRICE * o.QTY * (1 - o.DISCOUNT)) over(partition by COMPANYNAME) as gross_sales FROM SALES.ORDERDETAILS o 
  join PRODUCTION.PRODUCTS p
  on o.PRODUCTID = p.PRODUCTID
  join PRODUCTION.SUPPLIERS s
  on p.SUPPLIERID = s.SUPPLIERID
  where o.ORDERID IN (SELECT ORDERID FROM SALES.ORDERS where YEAR(ORDERDATE) = 2015))
  join (
    SELECT distinct s.COMPANYNAME as cn, sum(o.UNITPRICE * o.QTY * (1 - o.DISCOUNT)) over(partition by COMPANYNAME) as gross_sales2 FROM SALES.ORDERDETAILS o 
    join PRODUCTION.PRODUCTS p
    on o.PRODUCTID = p.PRODUCTID
    join PRODUCTION.SUPPLIERS s
    on p.SUPPLIERID = s.SUPPLIERID
    where o.ORDERID IN (SELECT ORDERID FROM SALES.ORDERS where YEAR(ORDERDATE) = 2014)) temp1
  on temp1.cn = COMPANYNAME;


    
-- c) omeji izpis s samo tistimi dobavitelji, ki so imeli več kot +10k razlike v prodaji
select COMPANYNAME, GROSS_SALES, (GROSS_SALES - GROSS_SALES2) as abs_change 
from (SELECT distinct s.COMPANYNAME, sum(o.UNITPRICE * o.QTY * (1 - o.DISCOUNT)) over(partition by COMPANYNAME) as gross_sales FROM SALES.ORDERDETAILS o 
  join PRODUCTION.PRODUCTS p
  on o.PRODUCTID = p.PRODUCTID
  join PRODUCTION.SUPPLIERS s
  on p.SUPPLIERID = s.SUPPLIERID
  where o.ORDERID IN (SELECT ORDERID FROM SALES.ORDERS where YEAR(ORDERDATE) = 2015))
  join (
    SELECT distinct s.COMPANYNAME as cn, sum(o.UNITPRICE * o.QTY * (1 - o.DISCOUNT)) over(partition by COMPANYNAME) as gross_sales2 FROM SALES.ORDERDETAILS o 
    join PRODUCTION.PRODUCTS p
    on o.PRODUCTID = p.PRODUCTID
    join PRODUCTION.SUPPLIERS s
    on p.SUPPLIERID = s.SUPPLIERID
    where o.ORDERID IN (SELECT ORDERID FROM SALES.ORDERS where YEAR(ORDERDATE) = 2014)) temp1
  on temp1.cn = COMPANYNAME
  where abs_change > 10000;
      

-- d) vzami rezultat iz točke a) in dodaj novi koloni - mesec in YTD gross revenue
SELECT distinct s.COMPANYNAME, sum(o.UNITPRICE * o.QTY * (1 - o.DISCOUNT)) over(partition by COMPANYNAME) as gross_sales FROM SALES.ORDERDETAILS o 
  join PRODUCTION.PRODUCTS p
  on o.PRODUCTID = p.PRODUCTID
  join PRODUCTION.SUPPLIERS s
  on p.SUPPLIERID = s.SUPPLIERID
  where o.ORDERID IN (SELECT ORDERID FROM SALES.ORDERS where YEAR(ORDERDATE) = 2015);


-- 2. NALOGA

-- a) izpiši celotno prodajo po letih za top 10 kupcev v letu 2014 za kategorijo "Beverages", ki so opravili vsaj 5 nakupov (production.categories -> categoryname)



  SELECT DISTINCT companyname, YEAR(orderdate) AS year, sum(unitprice * qty * (1 - discount)) over(partition by custid, YEAR(orderdate)) AS gross_sales, count(DISTINCT orderid) OVER (PARTITION BY custid, YEAR(orderdate)) AS purcheses FROM Sales.Orderdetails ord
  JOIN Sales.Orders
  USING (orderid)
  JOIN Sales.Customers
  USING (custid)
  RIGHT JOIN (SELECT DISTINCT custid, SUM(qty) OVER(PARTITION BY custid) AS Beverages FROM sales.orderdetails c
    JOIN sales.orders o
    USING (orderid)
    JOIN production.products p
    USING (productid)
    JOIN production.categories cg
    USING (categoryid)
    WHERE categoryname = 'Beverages' and YEAR(orderdate) = 2014
    ORDER BY Beverages DESC LIMIT 10)
  USING (custida);




-- b) izpiši kakšen delež predstavlja teh 10 kupcev po letih v celotni prodaji

SELECT  companyname, year, sum(gross_sales) over(PARTITION BY year) from (
  SELECT companyname, year, gross_sales FROM (
    SELECT DISTINCT companyname, YEAR(orderdate) AS year, sum(unitprice * qty * (1 - discount)) over(partition by custid, YEAR(orderdate)) AS gross_sales, count(DISTINCT orderid) OVER (PARTITION BY custid, YEAR(orderdate)) AS purcheses FROM Sales.Orderdetails ord
    JOIN Sales.Orders
    USING (orderid)
    JOIN Sales.Customers
    USING (custid)
    RIGHT JOIN (SELECT DISTINCT custid, SUM(qty) OVER(PARTITION BY custid) AS Beverages FROM sales.orderdetails c
      JOIN sales.orders o
      USING (orderid)
      JOIN production.products p
      USING (productid)
      JOIN production.categories cg
      USING (categoryid)
      WHERE categoryname = 'Beverages' and YEAR(orderdate) = 2014
      ORDER BY Beverages DESC LIMIT 10)
    USING (custid))
  WHERE purcheses>4)
  ;



-- *glede na to, da imamo 3 leta podatkov, mora imeti rezultat 6 vrstic--     


-- 3. NALOGA

-- a) izpiši za vsakega kupca koliko nakupov je v določenem razredu "trajanja med dvema nakupoma" -


SELECT companyname, sum(CASE WHEN diff < 11 THEN 1 ELSE 0 END) as "do_10",
                    sum(CASE WHEN 10 < diff < 51 THEN 1 ELSE 0 END) as "11-50",
                    sum(CASE WHEN 50 < diff < 101 THEN 1 ELSE 0 END) as "51-100",
                    sum(CASE WHEN 100 < diff THEN 1 ELSE 0 END) as "vec"
FROM (
  SELECT companyname, DATEDIFF(day, o1.orderdate, o2.orderdate) AS diff
  FROM (
    SELECT companyname, custid, orderdate, ROW_NUMBER() OVER(PARTITION BY custid ORDER BY orderdate) as n_order FROM sales.customers
      JOIN sales.orders
      USING (custid)
      ) o1
  JOIN (
    SELECT custid, orderdate, ROW_NUMBER() OVER(PARTITION BY custid ORDER BY orderdate) as n_order FROM sales.customers
      JOIN sales.orders
      USING (custid)
      ) o2
  ON (o1.custid = o2.custid AND o1.n_order+1 = o2.n_order))
group by companyname;



-- torej če je stranka opravila nakup 1.1.2020 in naslednji nakup 20.1.2020 pomeni, da je bilo med nakupoma 20 dni

-- štejemo samo tiste, ki so imeli vsaj 2 nakupa

-- predefinirani razredi - do 10 dni, med 11 in 50 dni, med 51 in 100 dni, več

-- izpis oblike:

-- companyname (sales.customers) | do_10 | 11-50 | 51-100| več
      
      

```