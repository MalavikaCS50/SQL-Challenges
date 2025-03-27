# SQL-Challenges

**#Amazon Data Analyst Interview Question – SQL Challenge**

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



**#Amazon Data Analyst Interview Question – SQL Challenge 2**

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



