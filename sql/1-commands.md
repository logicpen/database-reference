### Commands to interact with psql
* `psql` terminal comes with postgres to interact with postgres server.
* In docker we can use `docker exec -it container_name bash` to connect to the container bash shell and `psql -U postgres` to use psql terminal.
* We can use `exit` command to exit from psql terminal as well as docker container bash shell. `\q` command can also be used to exit from psql terminal.
* After connecting with `psql`, we can perform various operations via different commands such as,
    * `\l` - list all the databases
    * `\c dbname` - use a particular database
    * `\d` - describe db