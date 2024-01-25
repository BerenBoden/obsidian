### MySQL / MariaDB

- **Create User**:
    
    sqlCopy code
    
    `CREATE USER 'username'@'host' IDENTIFIED BY 'password';`
    
- **Grant Privileges**:
    
    sqlCopy code
    
    `GRANT ALL PRIVILEGES ON database_name.table_name TO 'username'@'host';`
    
    You can replace `ALL PRIVILEGES` with specific privileges like `SELECT`, `INSERT`, `UPDATE`, `DELETE`, etc.
    
- **Grant All Privileges on All Databases**:
    
    sqlCopy code
    
    `GRANT ALL PRIVILEGES ON *.* TO 'username'@'host';`
    
- **Revoke Privileges**:
    
    sqlCopy code
    
    `REVOKE privilege_name ON database_name.table_name FROM 'username'@'host';`
    
- **Show Grants for User**:
    
    sqlCopy code
    
    `SHOW GRANTS FOR 'username'@'host';`
    
- **Drop User**:
    
    sqlCopy code
    
    `DROP USER 'username'@'host';`
    

### PostgreSQL

- **Create User**:
    
    sqlCopy code
    
    `CREATE USER username WITH PASSWORD 'password';`
    
- **Grant Privileges**:
    
    sqlCopy code
    
    `GRANT SELECT, INSERT, UPDATE, DELETE ON table_name TO username;`
    
    You can list specific privileges as needed.
    
- **Grant All Privileges on All Tables in a Schema**:
    
    sqlCopy code
    
    `GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO username;`
    
- **Alter Default Privileges** (for new tables):
    
    sqlCopy code
    
    `ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO username;`
    
- **Revoke Privileges**:
    
    sqlCopy code
    
    `REVOKE ALL PRIVILEGES ON table_name FROM username;`
    
- **Drop User**:
    
    sqlCopy code
    
    `DROP USER username;`
    

### SQL Server

- **Create User**:
    
    sqlCopy code
    
    `CREATE LOGIN username WITH PASSWORD = 'password'; CREATE USER username FOR LOGIN username;`
    
- **Grant Privileges**:
    
    sqlCopy code
    
    `GRANT SELECT, INSERT, UPDATE, DELETE ON table_name TO username;`
    
    You can specify the privileges as needed.
    
- **Revoke Privileges**:
    
    sqlCopy code
    
    `REVOKE SELECT, INSERT, UPDATE, DELETE ON table_name FROM username;`
    
- **Drop User**:
    
    sqlCopy code
    
    `DROP USER username; DROP LOGIN username;`
    

### Oracle

- **Create User**:
    
    sqlCopy code
    
    `CREATE USER username IDENTIFIED BY password;`
    
- **Grant Privileges**:
    
    sqlCopy code
    
    `GRANT SELECT, INSERT, UPDATE, DELETE ON table_name TO username;`
    
- **Grant All Privileges on All Tables**:
    
    sqlCopy code
    
    `GRANT ALL PRIVILEGES TO username;`
    
- **Revoke Privileges**:
    
    sqlCopy code
    
    `REVOKE SELECT, INSERT, UPDATE, DELETE ON table_name FROM username;`
    
- **Drop User**:
    
    sqlCopy code
    
    `DROP USER username CASCADE;`
##### ***Resources***
