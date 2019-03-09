# DatabaseAssignment

q1:

with empTable as (select employeeNumber, city as 'empCity' from employees 
inner join offices on employees.officeCode = offices.officeCode) 

select customerNumber, employeeNumber, city, empCity from customers 
inner join empTable on customers.salesRepEmployeeNumber = empTable.employeeNumber where city = empCity;

q2: CREATE INDEX city ON customers(city);

before
<img src="https://github.com/Hallur20/DatabaseAssignment6/blob/master/before_indexes_ex1.png"/>
after
<img src="https://github.com/Hallur20/DatabaseAssignment6/blob/master/after_indexes_ex2.png"/>

q3:

group by:

SELECT employees.officeCode, SUM(quantityOrdered * priceEach) AS totalPrice from orderdetails 
inner join orders on orderdetails.orderNumber = orders.orderNumber
inner join customers on orders.customerNumber = customers.customerNumber
inner join employees on customers.salesRepEmployeeNumber = employees.employeeNumber
group by employees.officeCode;
