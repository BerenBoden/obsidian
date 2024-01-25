##### ***SQL permission commands***
**Create User**
```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

**Grant Privileges**
```sql
GRANT ALL PRIVILEGES ON database_name.table_name TO 'username'@'host';
```

**Grant All Privileges on All Databases**
```sql
GRANT ALL PRIVILEGES ON *.* TO 'username'@'host';
```

**Revoke Privileges**
```sql
REVOKE privilege_name ON database_name.table_name FROM 'username'@'host';
```