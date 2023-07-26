### Database Transactions
* Database transactions are single unit of work that may contain more than one intermediate steps.
* Let's say during a money transfer customer A transfers money to customer B. So we have two steps here deduct money form A's account and add the same to B's account. Let's say after we deduct money form A's account there was some issue so we couldn't add the money to B's account.
* In the above case we should refund the money deducted to A's account.
* So deducting from A's account adding to B's account can seen as a single unit of work i.e we want these two tasks to be done successfully. Incase there's any error while doing these tasks we should able to revert back to the original state.
* As the two tasks above described are bound to be carried out with successfully together, it can be refer as a transaction.
* `BEGIN` is used to start a transaction, `COMMIT` is used to end the transaction if the the intermediate operations are successfully carried out, `ROLLBACK` is used if any of the intermediate steps fails.
```sql
-- start a transaction
BEGIN;

-- deduct money from A's account
-- sql code goes here

-- add money to B's account
-- sql code goes here

-- end the transaction if success
COMMIT;
```
```sql
-- start a transaction
BEGIN;

-- deduct money from A's account
-- sql code goes here

-- add money to B's account
-- sql code goes here

-- end the transaction if fails
ROLLBACK;
```
