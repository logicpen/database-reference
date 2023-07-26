### Aggregate functions
* Aggregate functions perform a calculation on a set of rows and return a single row.
* Below table would be used to demonstrate the queries
```
customer_id |   name   | age | gender | address 
-----------------------------------------------
         1  | Jack     |  25 | male   | London
         2  | Jill     |  21 | female | Chicago
         3  | Tom      |  27 | male   | Austin
         4  | Samantha |  29 | female | Boston
         5  | Nick     |  19 | male   | Miami

 order_id |customer_id  |  item  | quantity 
--------------------------------------------
        1 |           1 | Book   |        2
        2 |           1 | Watch  |        1
        3 |           3 | Bag    |        1
        4 |           4 | Jeans  |        3
        5 |           5 | Bottle |        5
```

* `AVG` function is used to find average of a particular column. 
```sql
SELECT 
	AVG (age) AS avg_age -- calculate avg age
FROM
	customers;
```
**output**
```
       avg_age       
---------------------
 24.2000000000000000
```
* `MAX` function is used to find max of a particular column. 
```sql
SELECT 
	MAX( age) AS max_age
FROM
	customers;
```
**output**
```
 max_age 
---------
    29
```
* `MIN` function is used to find max of a particular column. 
```sql
SELECT 
	MIN (age) AS min_age
FROM
	customers;
```
**output**
```
 min_age 
---------
    19
```
* `SUM` function is used to find total of a particular column. 
```sql
SELECT 
	SUM(quantity) AS total_quantity --total quantity ordered by all customers
FROM
	orders;
```
**output**
```
 total_quantity 
----------------
      12
```
#### GROUP BY 
* This clause is used to group the rows that has identical data. In the below query we are grouping `customer_id` and making a total of order quantity.
`customer_id` 1 has ordered total 3 no of items in 2 times so we grouped all the rows that have same `customer_id` made one row.  
```sql
SELECT 
	customer_id, SUM(quantity) AS total_quantity
FROM
	orders
GROUP BY 
    customer_id
ORDER BY 
	customer_id;
```
**output**
```
 customer_id | total_quantity 
-------------+----------------
        1    |           3
        3    |           1
        4    |           3
        5    |           5 
```
#### HAVING
* `HAVING` clause is usually used with `GROUP BY` clause to apply condition on the group of rows that has been generated by `GROUP BY` clause.
```sql
 SELECT 
	customer_id, SUM(quantity) AS total_quantity
FROM
	orders
GROUP BY 
    customer_id
HAVING
	SUM(quantity) > 2
ORDER BY 
	customer_id;
```
**output**
```
 customer_id | total_quantity 
-------------+----------------
        1    |              3
        4    |              3
        5    |              5
```
* In the above query we can see that after we group all the rows with same `customer_id` and total order quantity we filtered for the rows which has order quantity greater than 2.
#### Examples
* Returning total number of orders for each customer with customer data where total quantity is greater than 2.
```sql
SELECT 
	customers.customer_id, customers.name, customers.address,
	SUM(orders.quantity) AS total_quantity
FROM
	customers, orders
WHERE
	customers.customer_id = orders.customer_id
GROUP BY 
    customers.customer_id, customers.name
HAVING
	SUM(quantity) > 2
ORDER BY 
	customer_id;
```
**output**
```
 customer_id |   name   | address | total_quantity 
-------------+----------+---------+----------------
        1    | Jack     | London  |              3
        4    | Samantha | Boston  |              3
        5    | Nick     | Miami   |              5
```