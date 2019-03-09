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
SELECT employees.officeCode,SUM(quantityOrdered * priceEach) AS totalPrice, max((quantityOrdered * priceEach)) as maxPrice from orderdetails 
inner join orders on orderdetails.orderNumber = orders.orderNumber
inner join customers on orders.customerNumber = customers.customerNumber
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber
group by employees.officeCode
```
<h3>Using windowing</h3>

<h1>Excercise 4</h1>

```sql
select displayName, title from users inner join posts on posts.OwnerUserId = users.id 
where title like '%grounds%';
```
