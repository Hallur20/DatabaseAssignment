# DatabaseAssignment6

<h1>Excercise 1</h1>

```sql
with empTable as (select employeeNumber, city as 'empCity' from employees 
inner join offices on employees.officeCode = offices.officeCode) 
select customerNumber, employeeNumber, city, empCity from customers 
inner join empTable on customers.salesRepEmployeeNumber = empTable.employeeNumber where city = empCity;
```
<h1>Excercise 2</h1> 

```sql
CREATE INDEX city ON customers(city);
```
<h3>Before</h3>

<img src="https://github.com/Hallur20/DatabaseAssignment6/blob/master/before_indexes_ex1.png"/>

<h3>After</h3>

<img src="https://github.com/Hallur20/DatabaseAssignment6/blob/master/after_indexes_ex2.png"/>

<h3>Conclusion</h3>

The costs went down from 39 to 7 when we created a index in customer table on city column.

<h1>Excercise 3</h1>

<h3>Using grouping</h3>

<p>We believe this is the correct solution where we choose amount from payments table</p>

```sql
SELECT employees.officeCode,SUM(amount) AS totalPrice, max(amount) as maxPrice from payments 
inner join customers on payments.customerNumber = customers.customerNumber 
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber
group by employees.officeCode
```

<p>If you disagree and you think that orderdetails should be used instead we have setup this aswell</p>


```sql
SELECT employees.officeCode,SUM(quantityOrdered * priceEach) AS totalPrice, max((quantityOrdered * priceEach)) as maxPrice from orderdetails 
inner join orders on orderdetails.orderNumber = orders.orderNumber
inner join customers on orders.customerNumber = customers.customerNumber
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber
group by employees.officeCode
```

<h3>Using windowing</h3>

<p>We believe this is the correct solution where we choose amount from payments table</p>

```sql
select distinct sum(amount) over (partition by employees.officeCode) as 'total sold', officeCode, max(amount) over (partition by employees.officeCode) as 'maximum payment' from payments 
inner join customers on payments.customerNumber = customers.customerNumber 
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber;
```
<p>If you disagree and you think that orderdetails should be used instead we have setup this aswell</p>

```sql
SELECT distinct sum(quantityOrdered * priceEach) over (partition by employees.officeCode) as 'total sold', officeCode, max(quantityOrdered * priceEach) over (partition by employees.officeCode) as 'maximum payment' from orderdetails 
inner join orders on orderdetails.orderNumber = orders.orderNumber
inner join customers on orders.customerNumber = customers.customerNumber
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber
```

<h1>Excercise 4</h1>

```sql
select displayName, title from users inner join posts on posts.OwnerUserId = users.id 
where title like '%grounds%';
```

<h1>Exercise 5</h1>

```sql
CREATE FULLTEXT INDEX index_name
ON posts(title)
```

<p>In order to use the created fulltext index key "index_name", you should change the query
  (from excerice 4) into this:</p>

```sql
select displayName, title from users inner join posts on posts.OwnerUserId = users.id 
where match(title) against ('grounds' IN BOOLEAN MODE)
```
<h3>Before</h3>

<img src="https://github.com/Hallur20/DatabaseAssignment6/blob/master/ex4_before.PNG"/>

<h3>After</h3>

<img src="https://github.com/Hallur20/DatabaseAssignment6/blob/master/ex4_after.png"/>

<h3>Conclusion</h3>

After creating the full text index the costs went down,you can notice the change from the provided screenshots which they show that the new qurey from excercise 5 does not make a full scan on the post table as the qurey from excercise 4


