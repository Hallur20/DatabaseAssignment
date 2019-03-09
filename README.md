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

<h1>Excercise 3</h1>

<h3>Using grouping</h3>

```sql
SELECT employees.officeCode,SUM(amount) AS totalPrice, max(amount) as maxPrice from payments 
inner join customers on payments.customerNumber = customers.customerNumber 
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber
group by employees.officeCode
```

```sql
SELECT employees.officeCode,SUM(quantityOrdered * priceEach) AS totalPrice, max((quantityOrdered * priceEach)) as maxPrice from orderdetails 
inner join orders on orderdetails.orderNumber = orders.orderNumber
inner join customers on orders.customerNumber = customers.customerNumber
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber
group by employees.officeCode
```

<h3>Using windowing</h3>

```sql
select distinct sum(amount) over (partition by employees.officeCode) as 'total sold', officeCode, max(amount) over (partition by employees.officeCode) as 'maximum payment' from payments 
inner join customers on payments.customerNumber = customers.customerNumber 
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber;
```

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

<p>we now have to use another syntax in exercise 4 in order to use this fulltext index key:</p>

```sql
select displayName, title from users inner join posts on posts.OwnerUserId = users.id 
where match(title) against ('grounds' IN BOOLEAN MODE)
```

before <img src="https://github.com/Hallur20/DatabaseAssignment6/blob/master/ex4_before.PNG"/>
after <img src=""/>
