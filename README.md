# DatabaseAssignment

q1: with empTable as (select employeeNumber, city as 'empCity' from employees 
inner join offices on employees.officeCode = offices.officeCode) 

select customerNumber, employeeNumber, city, empCity from customers 
inner join empTable on customers.salesRepEmployeeNumber = empTable.employeeNumber where city = empCity;

q2: CREATE INDEX city ON customer(city);
