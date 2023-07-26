### Joins
* `Joins` are used to combine columns from different table. Usually these tables are inter related i.e primary key of one table is foreign of another table.
* We can combine different column of a single table in that case it would be called as `self join`.
#### Inner Join
* Inner join joins two table depending upon the condition and returns the value of the result.
```sql
SELECT
	customers.name, customers.address, orders.item, orders.quantity
FROM
	customers
INNER JOIN 
	orders
ON
	customers.customer_id = orders.customer_id;
```
**output**
```
   name   | address |  item  | quantity 
----------+---------+--------+----------
 Jack     | London  | Book   |        2
 Jack     | London  | Watch  |        1
 Tom      | Austin  | Bag    |        1
 Samantha | Boston  | Jeans  |        3
 Nick     | Miami   | Bottle |        5
```
* We can see in the above query we joined `customers` and `orders` table. We used primary key of customers table(`customers.customer_id`) and foreign key of `orders` table(`orders.customer_id`).
#### Left Join
* Left join returns all rows from the LEFT-hand table specified in the `ON` condition and only those rows from the other table where the joined fields are equal (join condition is met).
```sql
SELECT
	customers.name, customers.address, orders.item, orders.quantity
FROM
	customers
LEFT JOIN 
	orders
ON
	customers.customer_id = orders.customer_id;
```
**output**
```
   name   | address |  item  | quantity 
----------+---------+--------+----------
 Jack     | London  | Book   |        2
 Jack     | London  | Watch  |        1
 Tom      | Austin  | Bag    |        1
 Samantha | Boston  | Jeans  |        3
 Nick     | Miami   | Bottle |        5
 Jill     | Chicago |        |         
```
* In the above query we see that all the rows from `customers` table have retrieved because it's on the left side of the `ON` condition. As customer named `Jill` doesn't have any orders, so the item and quantity field for `Jill` has shown empty.
#### Right Join
* * Right join returns all rows from the RIGHT-hand table specified in the `ON` condition and only those rows from the other table where the joined fields are equal (join condition is met).
```sql
SELECT
	customers.name, customers.address, orders.item, orders.quantity
FROM
	customers
RIGHT JOIN 
	orders
ON
	customers.customer_id = orders.customer_id;
```
**output**
```
   name   | address |  item  | quantity 
----------+---------+--------+----------
 Jack     | London  | Book   |        2
 Jack     | London  | Watch  |        1
 Tom      | Austin  | Bag    |        1
 Samantha | Boston  | Jeans  |        3
 Nick     | Miami   | Bottle |        5
```
* We can see from the above query that all the rows from `orders` table have retrieved because it's on the left side of the `ON` condition. As customer named `Jill` doesn't have any orders, so it has been left out form the final result.
#### Full Outer Join
* This join would return all the rows from both the table. If the any of the rows doesn't satisfy the given condition the respective fields would be left empty or null.
```sql
SELECT
	customers.name, customers.address, orders.item, orders.quantity
FROM
	customers
FULL JOIN 
	orders
ON
	customers.customer_id = orders.customer_id;
```
**output**
```
  name   | address |  item  | quantity 
----------+---------+--------+----------
 Jack     | London  | Book   |        2
 Jack     | London  | Watch  |        1
 Tom      | Austin  | Bag    |        1
 Samantha | Boston  | Jeans  |        3
 Nick     | Miami   | Bottle |        5
 Jill     | Chicago |        |         
```
* We can see form all the above examples that joins don't merge columns which has identical data. Jack has two orders so while joining the final result would contain two rows for two columns.
### Examples 
* Finding all the customers and their order details where the age is greater than 22.
```sql
SELECT
	customers.customer_id, customers.name, customers.age, customers.address, orders.item, orders.quantity
FROM
	customers
INNER JOIN 
	orders
ON
	customers.customer_id = orders.customer_id
WHERE
	age > 22;
```
**output**
```
 customer_id |   name   | age | address | item  | quantity 
-------------+----------+-----+---------+-------+----------
        1    | Jack     |  25 | London  | Book  |        2
        1    | Jack     |  25 | London  | Watch |        1
        3    | Tom      |  27 | Austin  | Bag   |        1
        4    | Samantha |  29 | Boston  | Jeans |        3
```
