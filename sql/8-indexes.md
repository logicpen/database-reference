### Indexes
* Searching data form a large dataset in a database can be time consuming. Now days enterprises deal with billions of entries in database.
* Index are used to increase the performance of search queries in database. It's similar to indexes at the back of a book where topics are arranged in alphabetical order and then the page number, so if we need to look up for a particular topic with respect to the alphabet then we can directly check the page where the topic is discussed in details.
* When we create index for a particular column, the database server automatically creates an lookup table for that column. This lookup table is used by the database search engine to search entries from the table.
* Even though `SELECT` and `WHERE` queries performs better with index, however `INSERT` and `UPDATE` queries performs slower with the index. Also indexes add storage overhead to the database systems. So indexes should be created with utmost care.
 