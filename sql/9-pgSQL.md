### PL/pgSQL
* `PL/pgSQL` is procedural language that gives many procedural elements e.g., control structures, loops, and complex computations to extend standard SQL.
* It allows to develop complex functions and stored procedures in PostgreSQL that may not be possible using plain SQL.
#### String
* In `PL/pgSQL`, we can use `$tag$` to use declare a string constant. `tag` is optional and can contain zero or any number of characters.
* Strings can also be declared by using single quote but if we have another singe quote inside the string constant we have to escape it by doubling the single quote.
```sql
SELECT 'I''m a string constant' AS string_constant; -- use double single quote to escape
```
**output**
```
    string_constant    
-----------------------
 I'm a string constant
```
```sql
-- two dollar signs without any characters inside it
SELECT $$I'm a string constant.$$ AS string_constant;
```
**output**
```
    string_constant     
------------------------
 I'm a string constant.
```
### PL/pgSQL Block Structure
* `PL/pgSQL` is a block Structured language.
* Each block has two sections, declaration section starts with `DECLARE` and a body section which starts with `BEGIN` and ends with `END`.
Declaration section is optional and body section is mandatory.
* We can specify a label(optional) to a block like `<<first_block>>` in the below example.
* The declaration section is where we declare all variables used within the body section in this case `customer_count`.
* `DO` statement does not belong to the block. It is used to execute an anonymous block. We used dollar quoted string constant to make it more readable.
```sql
DO $$ 
<<first_block>>
DECLARE
  customer_count INTEGER := 0; -- declaring a variable(= can be used for assignment)
BEGIN
   -- get the number of users(body section)
   SELECT COUNT(*) -- count all the customers in customer table
   INTO customer_count -- assign total count to the variable after counting
   FROM customers;
   -- display a message
   RAISE NOTICE 'The number of customers is %', customer_count; -- % is a place holder of variable customer_count
END first_block $$;
```
**output**`NOTICE:  The number of customers is 5` 
### Variable

to do 

### Control Structure
* There are many ways we can add control statements
#### If-then, elseif-then, else :-


#### Case-when :-


#### 
