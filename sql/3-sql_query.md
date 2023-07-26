### SQL queries
---
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
* Above are two tables `customer_id` in orders table is a foreign key which references to `customer_id` in customers table.
#### SELECT Statement
* Below query would give all the entries from a `customers` table.
```sql
SELECT 
    * 
FROM
    customers;
```
**output**
```
customer_id |   name   | age | gender | address 
-----------------------------------------------
         1  | Jack     |  25 | male   | London
         2  | Jill     |  21 | female | Chicago
         3  | Tom      |  27 | male   | Austin
         4  | Samantha |  29 | female | Boston
         5  | Nick     |  19 | male   | Miami
```
* Below query would give all the entries from a `customers` table with specific columns(in this case name, age and gender).
```sql
SELECT 
	name, age, gender 
FROM 
	customers;
```
**output**
```
   name   | age | gender 
-------------------------
 Jack     |  25 | male
 Jill     |  21 | female
 Tom      |  27 | male
 Samantha |  29 | female
 Nick     |  19 | male
```
* We can concatenate two columns and create a new column with new name. In below example we created new column `name_address`(alias) by concatenating name and address column.
```sql
SELECT 
	name || '->' || address AS name_address
FROM 
	customers;
```
**output**
```
name_address   
------------------
 Jack->London
 Jill->Chicago
 Tom->Austin
 Samantha->Boston
 Nick->Miami
```
* We can order entries in ascending(`ASC`) and descending(`DESC`) order with respect to a particular column. It can be used for both numerical and string type columns.
```sql
SELECT 
	customer_id, name, age, address
FROM 
	customers
ORDER BY
	customer_id DESC;
```
**output**
```
 customer_id |   name   | age | address 
-------------+----------+-----+---------
           5 | Nick     |  19 | Miami
           4 | Samantha |  29 | Boston
           3 | Tom      |  27 | Austin
           2 | Jill     |  21 | Chicago
           1 | Jack     |  25 | London
```
* We can remove duplicates while querying entries. Following query would give which customers have ordered. customer_id even if `customer_id` 1 has 2 orders but would appear one time.
```sql
SELECT 
	DISTINCT customer_id
FROM 
	orders;
```
**output**
```
customer_id 
-------------
    3
    5
    4
    1
```
### WHERE clause
* We can use operators with `WHERE` clause to filter entries.

|Operator |Description   |
|---------|--------------|
| =       | Equal        |
| >       | Greater than |
| <       | Less than    |
| >=      | Greater than or Equal|
| <=      | Less than or Equal  |
| !=      | Not equal    |
| AND     | Logical and  |
| OR      | Logical OR   |
| IN      | Returns rows which matches any values from a given list of values|
| BETWEEN | Returns rows present in range of values|
| LIKE    | Gives values which matches the pattern|

* `OR` operator  to find if there is any customer with name Jack or Jill
```sql
SELECT * 
FROM
	customers
WHERE 
	name = 'Jack' OR
	name = 'Jill';
```
**output**
```
 customer_id | name | age | gender | address 
-------------+------+-----+--------+---------
           1 | Jack |  25 | male   | London
           2 | Jill |  21 | female | Chicago
```
* `AND` operator to find a customer whose age is greater than 25 and gender is female.
```sql
SELECT * 
FROM
	customers
WHERE 
	age > 25 AND
	gender = 'female';
```
**output**
```
 customer_id |   name   | age | gender | address 
-------------+----------+-----+--------+---------
           4 | Samantha |  29 | female | Boston
```
* `IN` operator to find if there are any entries which matches with given list of values.
```sql
SELECT * 
FROM
	customers
WHERE 
	name IN('Jack','Tom', 'Will');
```
**output**
```
customer_id | name | age | gender | address 
-------------+------+-----+--------+---------
           1 | Jack |  25 | male   | London
           3 | Tom  |  27 | male   | Austin
```
* `BETWEEN` operator to find entires that satisfies a range of values.
```sql
SELECT * 
FROM
	customers
WHERE 
	age BETWEEN 20 AND 26; -- all the customer whose age falls between and including 20 and 26
```
**output**
```
 customer_id | name | age | gender | address 
-------------+------+-----+--------+---------
        1    | Jack |  25 | male   | London
        2    | Jill |  21 | female | Chicago
```
* `LIKE` is used to match string pattern with a specified column.
```sql
SELECT * 
FROM
	customers
WHERE 
	name LIKE '%il%'; -- matches any name that has il in it
```
**output**
```
 customer_id | name | age | gender | address 
-------------+------+-----+--------+---------
        2    | Jill |  21 | female | Chicago
```
**Examples**
```sql
SELECT * 
FROM
	customers
WHERE 
	age > 20 AND -- customers with age greater than 20 and has J in their name
	name LIKE '%J%';
```
**output**
```
 customer_id | name | age | gender | address 
-------------+------+-----+--------+---------
        1    | Jack |  25 | male   | London
        2    | Jill |  21 | female | Chicago
```
### LIMIT and OFFSET clause
* `LIMIT` is used to select number of rows and `OFFSET` is used to skip number of rows from the beginning. The below query would give 2 rows and will skip the first row of the table.
```sql
SELECT * 
FROM
	customers
LIMIT 2
OFFSET 1; -- skip customer_id 1
```
**output**
```
 customer_id | name | age | gender | address 
-------------+------+-----+--------+---------
        2    | Jill |  21 | female | Chicago
        3    | Tom  |  27 | male   | Austin
```