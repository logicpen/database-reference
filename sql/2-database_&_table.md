### Databases and Tables
---
* Tables are two dimensional structure that contains data in form of rows and columns. As an example a person table can contain different columns like name, age etc. 

|name|age|gender|address|
|----|---|------|-------|
|Jack| 25| male |London |
|Jill|21 |female|New York|
|Sam |23 |male  |Singapore|

* Database is something which stores multiple tabes as well as other objects such as stored procedures, triggers ,functions, Indexes, views etc.
* So at first we need to create a database and after that we would be able to create tables, indexes, functions etc.

### Basic Database operations
---
* `CREATE DATABASE dbname;` would create a database with given name. `CREATE DATABASE testdb;` would create a database named `testdb` in database server.
* `DROP DATABASE dbname;` wold delete a database with given name. `DROP DATABASE testdb;` would delete a database named `testdb` from database server.
### Basic Table operations
---
* We can create a table with following command(use `\d` to describe the database which would show all the tables present in the database and `\d table_name` to describe the specified table)
```sql
CREATE TABLE person (
    id SERIAL,
    name TEXT,
    age INT
);
```
* The above command would create a table with three columns i.e `id`, `name` and `age`. We can also see that each of these columns are of different data types.
* `id ` column is of type `SERIAL` which would start from 1 and increment automatically as we start adding rows to the table. `TEXT`(name column) data type is used to store textual(string) data, in this case name of a person. There are other types exist to store string types too. And lastly `INT`(age column) to store integers in this case age of a person.
* We can delete a table by the command `DROP TABLE table_name;`.
* Once the table is created we would be able to insert data to the table, update an existing data and also delete an existing data.
```sql
INSERT INTO 
    person (name, age)
VALUES
    ('John', 25);
```
* As `id` will increment starting from 1, we don't need to mention `id` column manually.
* We can add multiple rows to the table.
```sql
INSERT INTO 
    person (name, age)
VALUES
    ('John', 25), -- use single quote for string
    ('Jack', 23),
    ('Jill', 21);

```

### Data types
---
* Postgres provides different data types to use while creating tables. We can use these data types depending upon the data we want to store in a particular column.
* `boolean` data type is used to store true or false in a column. Let's say we want to store is if a person is a minor or not, so we can add an extra column like `isMinor BOOLEAN`.
* `VARCHAR(n)`, `CHAR(n)`, `TEXT` etc are used to store string value. `TEXT` is used to store string with unlimited length. Other two are used to store string up to specified length i.e n(always positive)
* `NUMERIC(precision, scale)` is used to store floating points. `precision` for total number of digits and `scale` is for number of digits after the decimal.
Example:- `SALARY NUMERIC(9, 3)`
* To store integer data type we can use `column_name INT` while creating a table.
* `DATE` store date value i.e adding a column named joining_date `joining_date DATE`
* We can store `UUID` in a column. `UUID` generator is not provided by default in postgres. We need to install an extension after that we can use the functions provided by the extension to generate uuid.
* We can also store `JSON` in a column. Syntax - `info JSON`(info column is of type JSON)
* we can store array of different types. Syntax - `phones TEXT []`(will store multiple texts)

### Constraints
---
* Constraints are a way to impose rules on a particular column. For an example, while creating an row in a table, we want some of the columns should not be empty.
* Let's say to add an user to user table, we need `user_name` and `password` to be field for sure while other fields such as `age`, `address` can be filled later.
* Again we want `user_name` to be unique i.e it shouldn't match with other `user_name`.
* We can add these constraints to our tables via database constraints. Different constraints are listed bellow. 
1. #### PRIMARY KEY :-
* `PRIMARY KEY` are used to identify a row uniquely in a table. Let's say in `user` table `phone_no` can be used as `PRIMARY KEY` because it's unique for each user and it can't be null.
* A table can't have multiple `PRIMARY KEY` but we can have unique combination of two columns. We can combine both `phone_no` and `email` and make it `PRIMARY KEY`. In this case postgres would combine `phone_no` and `email` to make unique pair.(`+123456789`, `test@email.com` and `+123456789`, `test@email.com` are unique pairs)
```sql
CREATE TABLE users (
	phone_no INTEGER PRIMARY KEY,
    email VARCHAR(20),
	name TEXT,
    age INT
);
-- another way to create PRIMARY KEY
CREATE TABLE users (
	phone_no INTEGER,
    email VARCHAR(20),
	name TEXT,
    age INT,
    PRIMARY KEY (phone_no, email) -- combining two columns to make primary key
);
-- ALTER TABLE can be used to add PRIMARY KEY to an existing table
CREATE TABLE users (
	phone_no INTEGER,
    email VARCHAR(20),
	name TEXT,
    age INT
);
ALTER TABLE users -- postgres would provide default constraint name
ADD PRIMARY KEY (phone_no, email);
-- same can be achieved 
ALTER TABLE users
  ADD CONSTRAINT users_pkey -- we specify constraint name
    PRIMARY KEY (phone_no, email);
-- deleting a PRIMARY KEY(ALTER TABLE is used to make changes to an existing table)
ALTER TABLE users
DROP CONSTRAINT users_pkey;
-- \d users wold give you the constraints in this case users_pkey.
```
2. #### CHECK :-
* This is used to check certain criteria before creating a row in a table. If we want each user's age should be greater than 18 then we can do the following
```sql
CREATE TABLE users (
	phone_no INTEGER PRIMARY KEY,
    email VARCHAR(20),
	name TEXT,
    age INT CHECK(age > 18)
);
-- if table is already exists without check constraints
ALTER TABLE users 
ADD CONSTRAINT age_check 
CHECK (
	age > 18
);
-- we can also do the following
ALTER TABLE users 
ADD CONSTRAINT age_check 
CHECK (
	age > 18
    AND age <= 60
);
```
3. #### UNIQUE :-
* `UNIQUE` constraint is used to make a column unique of a table. We want `email` to be unique for each user so we can do the following.
```sql
CREATE TABLE users (
	phone_no INTEGER PRIMARY KEY,
    email VARCHAR(20) UNIQUE,
	name TEXT,
    age INT CHECK(age > 18)
);
-- same can be achieved by following
CREATE TABLE users (
	phone_no INTEGER PRIMARY KEY,
    email VARCHAR(20),
	name TEXT,
    age INT CHECK(age > 18),
    UNIQUE(email) -- UNIQUE(email, phone_no) syntax can be used to make multiple unique columns
);
-- we can add unique constraints to an existing table
CREATE UNIQUE INDEX CONCURRENTLY users_email -- create a unique index based on the email column.
ON equipment (email);
ALTER TABLE equipment -- add a unique constraint to the users table using the users_email index.
ADD CONSTRAINT unique_equip_id 
UNIQUE USING INDEX equipment_equip_id;
```
4. #### NOT NULL :-
* `NOT NULL` constraint is used to create columns which can't left empty while adding a row. In the `users` table `email` and `name` can't be empty so following can be done while creating table.
```sql
CREATE TABLE users (
	phone_no INTEGER PRIMARY KEY,
    email VARCHAR(20) NOT NULL,
	name TEXT NOT NULL,
    age INT
);
-- we can add not null constraints for multiple columns in an existing table
ALTER TABLE users
ALTER COLUMN email SET NOT NULL,
ALTER COLUMN name SET NOT NULL;
```
4. #### FOREIGN KEY :-
* When we required to form a relationship between two tables, we use `FOREIGN KEY` constraint to do so.
* Let's say we have two tables named `customers` and `orders` to store customer information and order information of a particular customer respectively.
* `customers` table has `customer_id` column as `PRIMARY KEY`(unique and not null) and in `orders` table instead of storing whole customer information, we only use `customer_id` to refer to the customer the order belongs to.

|customer_id|name|age|gender|address  |  
|-----------|----|---|------|---------|
|1          |Jack|25 | male |London   | 
|2          |Jill|21 |female|New York |          
|3          |Sam |23 |male  |Singapore| 

|order_id|customer_id|item |quantity|
|--------|-----------|-----|--------|
|1       |1          |book |2       |
|2       |1          |fruit|3       |       
|3       |3          |cloth|1       |

* From the orders table we can say that `order_id` 1 and 2 belongs to `customer_id` 1 and `order_id` 3 belongs to `customer_id` 3. So `customer_id` column in `orders` table refers to the  `customer_id` column in `customers` table.
* `customer_id` column in `customers` table has to be the primary key so that it can be used as `FOREIGN KEY` constraint in `orders` table.
```sql
DROP TABLE IF EXISTS customers;
DROP TABLE IF EXISTS orders;

CREATE TABLE customers(
   customer_id INT,
   name VARCHAR(255) NOT NULL,
   age INT NOT NULL,
   gender VARCHAR(255) NOT NULL,
   address VARCHAR(255) NOT NULL,
   PRIMARY KEY(customer_id)
);

CREATE TABLE orders(
   order_id INT,
   customer_id INT,
   item VARCHAR(255) NOT NULL,
   quantity INT,
   PRIMARY KEY(order_id),
   CONSTRAINT fk_customer -- adding foreign key constraint
      FOREIGN KEY(customer_id) 
	  REFERENCES customers(customer_id)
);
```
* We can choose different column names (it doesn't have to be the same). Important thing is we should reference the column of one table to another table to create a foreign key constraint.
* As `orders` table references to `customers` table, we can see that for each order in `orders` table contains a valid `customer_id` which references to `customer_id` column in `customers` table. If we try to add a row with `customer_id` 4 in `orders` table, it would give error because there is no customer with `customer_id` 4.
* If we try to delete a customer with `customer_id` 3 from `customers` table, it would give an error because in the orders table `order_id` 3 still references to customer table with `customer_id` 3.
* To avoid last error caused due to deleting a customer which still is still referenced by `orders` table, we can use `ON DELETE` or `ON DELETE` action.
```sql
CONSTRAINT fk_customer -- adding foreign key constraint
    FOREIGN KEY(customer_id) 
	REFERENCES customers(customer_id)
    ON DELETE CASCADE
```
* If a customer is deleted from the `customers` table, this action would delete all the orders belongs to that customer from orders table. 