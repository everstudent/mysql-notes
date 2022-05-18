# Mysql
Mysql learning notes

### CLI
```bash
# connect
mysql -h host -P port -u user -p

# execute query from bash
mysql -e "SELECT NOW()"
```

### Helpers
```sql
SELECT USER(); -- current user
SELECT NOW(); -- current datetime
SELECT DATABASE(); -- current db
SHOW PROCESSLIST; -- running queries
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

TRUNCATE users; -- clean table
ALTER TABLE users ENGINE = MyISAM; -- change engine
```

### Inserts
```sql
INSERT INTO users
SET email = 'denys@student.com', name = 'Denys'
ON DUPLICATE KEY UPDATE
created_at = now();
```

### Agregates
```sql
SELECT name, count(*)
FROM users
WHERE email IS NOT NULL -- pre-group filter
GROUP BY name
HAVING name != 'John'; -- post-group filter
```

### Variables
```sql
SELECT @max_id:=MAX(id) FROM users;
SELECT * FROM users WHERE id <= @max_id;
```

### Foreign Keys
```sql
CREATE TABLE parent ( id INT NOT NULL, PRIMARY KEY (id) ) ENGINE=INNODB;

CREATE TABLE child (
    id INT,
    parent_id INT,
    INDEX par_ind (parent_id),
    FOREIGN KEY (parent_id) REFERENCES parent(id) -- creates foreign key to parent.id field
) ENGINE=INNODB;
```

### Triggers
```sql
delimiter //
CREATE TRIGGER user_name BEFORE INSERT ON users
     FOR EACH ROW
     BEGIN
       IF NEW.name IS NULL THEN
         SET NEW.email = NULL;
       END IF;
     END;//
delimiter ;

SHOW TRIGGERS;
```

### Common Table Expressions
```sql
CREATE TABLE int_sequence
WITH RECURSIVE sequence (n) AS
(
  SELECT 0
  UNION ALL
  SELECT n + 1 FROM sequence WHERE n + 1 <= 10
)
SELECT n
FROM sequence;

SELECT * FROM int_sequence; -- table with "n" column populated by 0...10 values
```
