# Mysql
Mysql learning notes

### CLI
```bash
# connect
mysql -h host -P port -u user -p

# execute query from bash
mysql -e "SELECT NOW()"
```

### Useful queries
```sql
SELECT USER(); -- current user
SELECT NOW(); -- current datetime
SELECT DATABASE(); -- current db
```

### Databases
```sql
CREATE DATABASE db DEFAULT CHARSET utf8mb4;
USE db;
SHOT TABLES; -- list all tables in this db
```

### Tables
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email varchar(128) not null,
  unique key(email),
  name varchar(256) not null,
  created_at datetime default now()
) engine = innodb, charset = utf8mb4;
```
