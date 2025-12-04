Since PostgreSQL is already installed and must NOT be restarted, we will only connect and run SQL commands.

✅ STEP 1 — Connect to PostgreSQL as postgres user
```
ssh user@db
sudo su - postgres # Switch to postgres user
psql               # Enter PostgreSQL shell
postgres=#         # You should now see
```
✅ STEP 2 — Create the database user
```
CREATE USER kloud_joy WITH PASSWORD 'YchZHRcLkL';
```
✅ STEP 3 — Create the database
```
CREATE DATABASE kloud_db6;
```
✅ STEP 4 — Grant all privileges on the database
```
GRANT ALL PRIVILEGES ON DATABASE kloud_db6 TO kloud_joy;
```
🔄 STEP 5 — Verify (Optional but recommended)
```
postgres=# \du
                                     List of roles
   Role name   |                         Attributes                         | Member of
---------------+------------------------------------------------------------+-----------
 kodekloud_joy |                                                            | {}
 postgres      | Superuser, Create role, Create DB, Replication, Bypass RLS | {}

postgres=# \l
                                  List of databases
     Name      |  Owner   | Encoding  | Collate | Ctype |     Access privileges
---------------+----------+-----------+---------+-------+----------------------------
 kodekloud_db6 | postgres | SQL_ASCII | C       | C     | =Tc/postgres              +
               |          |           |         |       | postgres=CTc/postgres     +
               |          |           |         |       | kodekloud_joy=CTc/postgres
 postgres      | postgres | SQL_ASCII | C       | C     |
 template0     | postgres | SQL_ASCII | C       | C     | =c/postgres               +
               |          |           |         |       | postgres=CTc/postgres
 template1     | postgres | SQL_ASCII | C       | C     | =c/postgres               +
               |          |           |         |       | postgres=CTc/postgres
(4 rows)
```
🔄 STEP 6 — Exit PostgreSQL shell
```
postgres=# \q
[postgres@db ~]$









