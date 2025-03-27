# SQL-Challenges

**1 #Amazon Data Analyst Interview Question – SQL Challenge**

In a recent Amazon data analyst interview, one of the candidates was given the following SQL challenge

Write a SQL query to calculate the average star rating for each product every month, rounded to two decimal places Sort the output based on month, followed by product_id.

![image](https://github.com/user-attachments/assets/dcfa7233-acf0-4740-8059-52e79cc9e78a)


**Code:**


CREATE TABLE review_table(
review_id INT PRIMARY KEY,
user_id INT,
submit_date TIMESTAMP,
product_id INT,
stars SMALLINT);

INSERT INTO review_table(review_id,user_id,submit_date,product_id,stars)
VALUES (6171,123,'06/08/2022 00:00:00',50001,4),
(7802,265,'06/10/2022 00:00:00',69852,4),
(5293,362,'06/18/2022 00:00:00',50001,3),
(6352,192,'07/26/2022 00:00:00',69852,3),
(4517,981,'07/05/2022 00:00:00',69852,2)

select * from review_table;

/*Write a SQL query to calculate the average star rating for each 
product every month, rounded to two decimal places Sort the output 
based on month, followed by product_id.*/

select
product_id,
EXTRACT(MONTH from submit_date) as month_num,
ROUND(AVG(stars),2) as avg_stars
from review_table
GROUP BY product_id,month_num
ORDER BY month_num, product_id



**2#Amazon Data Analyst Interview Question – SQL Challenge 2**

In a recent Amazon data analyst interview, one of the candidates was given the following SQL challenge

Consider a retail business scenario where you need to identify customers who have made purchases exceeding $100. You have two tables called customers (containing customer_id and name) and orders (containing order_id, customer_id, and order_amount). The task is to join these tables and calculate the total purchase amount for each customer and select customers whose total purchase amount exceeds $100.

![image](https://github.com/user-attachments/assets/8eaae8ee-8012-4c97-9fcc-19281a575e34)

**Code:**

CREATE TABLE customers_table(
customer_id SERIAL PRIMARY KEY,
name TEXT not null);

CREATE TABLE orders_table(
order_id SERIAL PRIMARY KEY,
customer_id SERIAL not null REFERENCES customers_table(customer_id),
order_amount FLOAT);

INSERT INTO customers_table(customer_id,name)
VALUES (1,'Alice'),
       (2,'Bob'),
	   (3,'Charlie'),
	   (4,'David')
	   
select * from customers_table;

INSERT INTO orders_table(order_id,customer_id,order_amount)
VALUES (1,1,50.00),
(2,2,75.00),
(3,3,100.00),
(4,1,120.00),
(5,2,80.00),
(6,4,150.00)

select * from orders_table;

/*Join the tables and calculate total purchase amnt for each cus
and select customers whose total exceed 100*/

/* To verify the answer for the final*/
select c.customer_id,o.order_id,o.order_amount
from customers_table c
left join orders_table o
ON c.customer_id=o.customer_id
ORDER BY c.customer_id

/*Final query*/
select c.customer_id,
sum(o.order_amount) as total_amount
from customers_table c
left join orders_table o
ON c.customer_id=o.customer_id
GROUP BY c.customer_id
HAVING sum(o.order_amount)>100
ORDER BY c.customer_id



**3 Beauty of SQL RANK Function | SQL Interview Question and Answers | Covid Cases**

Find the cities where the covid cases are increasing continuously

create table covid(city varchar(50),days date,cases int);
delete from covid;
insert into covid values('DELHI','2022-01-01',100);
insert into covid values('DELHI','2022-01-02',200);
insert into covid values('DELHI','2022-01-03',300);

insert into covid values('MUMBAI','2022-01-01',100);
insert into covid values('MUMBAI','2022-01-02',100);
insert into covid values('MUMBAI','2022-01-03',300);

insert into covid values('CHENNAI','2022-01-01',100);
insert into covid values('CHENNAI','2022-01-02',200);
insert into covid values('CHENNAI','2022-01-03',150);

insert into covid values('BANGALORE','2022-01-01',100);
insert into covid values('BANGALORE','2022-01-02',300);
insert into covid values('BANGALORE','2022-01-03',200);
insert into covid values('BANGALORE','2022-01-04',400);


**--Find the cities where the covid cases are increasing continuously**


**Code:**

WITH cte as(SELECT
*,
RANK() OVER (PARTITION BY city ORDER BY days asc) as rn_days,
RANK() OVER (PARTITION BY city ORDER BY cases asc) as rn_cases,
RANK() OVER (PARTITION BY city ORDER BY days asc)-RANK() OVER (PARTITION BY city ORDER BY cases asc) as diff
FROM covid)

select
city
from cte
group by city
having count(distinct diff)=1 and max(diff)=0


**4 Deadly Combination of Group By and Having Clause in SQL | SQL Interview Questions and Answers**


**--Find students with same marks in physics and chemistry**

CREATE TABLE exams(
student_id INT,
subject VARCHAR(20),
marks INT);

delete from exams;

INSERT INTO exams(student_id,subject,marks)
VALUES (1,'Chemistry',91),
(1,'Physics',91),
(2,'Chemistry',80),
(2,'Physics',90),
(3,'Chemistry',80),
(4,'Chemistry',71),
(4,'Physics',54);

select
student_id,
round(avg(marks),0) as avg_marks
from exams
WHERE subject IN ('Physics','Chemistry')
GROUP BY student_id
HAVING COUNT(DISTINCT marks)=1 and COUNT(DISTINCT subject)=2
ORDER BY student_id


**5 Double Self Join in SQL | Amazon Interview Question**


**--Find employee, manager, and senior manager from a single table**


**Code:**


create table emp(
emp_id int,
emp_name varchar(20),
department_id int,
salary int,
manager_id int,
emp_age int);

insert into emp
values
(1, 'Ankit', 100,10000, 4, 39),
(2, 'Mohit', 100, 15000, 5, 48),
(3, 'Vikas', 100, 12000,4,37),
(4, 'Rohit', 100, 14000, 2, 16),
(5, 'Mudit', 200, 20000, 6,55),
(6, 'Agam', 200, 12000,2, 14),
(7, 'Sanjay', 200, 9000, 2,13),
(8, 'Ashish', 200,5000,2,12),
(9, 'Mukesh',300,6000,6,51),
(10, 'Rakesh',500,7000,6,50)

select * from emp

select
e.emp_id,
e.emp_name,
m.emp_name as manager_name,
sm.emp_name as senior_manager
from emp e
LEFT JOIN emp m
ON e.manager_id = m.emp_id
LEFT JOIN emp sm
ON m.manager_id = sm.emp_id
ORDER BY e.emp_id



