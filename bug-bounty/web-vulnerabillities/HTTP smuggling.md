##### ***What is HTTP smuggling?***
HTTP smuggling is a web vulnerability that allows an attacker to interfere with the way a web server handles the stream of HTTP requests from users. With HTTP smuggling, an attacker be able to poison the sequence of HTTP requests to a web server by modifying the next request sent by a legitimate user to the back-end.
![image](https://beren-obsidian-images.imgix.net/194b27606fcf47e4c26baa48b9e22ec4.png)

This simplistic diagram represents the attacker appending arbitrary content to the start of the next users legitimate request. This is happening because the back-end server processes the request based on its understanding of the content-length and then sends a response back to the front-end server, thinking it has fully processed the request.

However, due to the conflicting content-length headers, the back-end server doesn't actually consume all the data that the front-end server sent. This leaves some residual data in the back-end server's buffer.

When the next legitimate request arrives at the back-end, it gets appended to this residual data. This can lead to unexpected behavior, including potential security vulnerabilities, because the back-end server might interpret this combined data in a way that the attacker can exploit.

So, to clarify, the attacker's request does get a response based on the first content-length header as understood by the back-end. But because of the conflicting content-length headers, some data is left over ("poisoning the socket"), which then interferes with the processing of subsequent legitimate requests.

Normally, requests with two content-length headers are blocked [RFC7320](https://datatracker.ietf.org/doc/html/rfc7230#section-3.3.3), however, chunked encoding will help us bypass this. According to [RFC2616](https://datatracker.ietf.org/doc/html/rfc2616#section-4.4) "If a message is received with both a Transfer-Encoding header field and a Content-Length header field, the latter MUST be ignored." This means if a request contains both the `Transfer-Encoding` and `Content-Length` headers, servers may accept this request.
##### ***HTTP smuggling techniques***
Here is how a very simple desynchronization technique works from a front-end request;
1. **Initial Request**: The attacker sends a POST request with both `Content-Length` and `Transfer-Encoding: chunked` headers. This is already unusual because typically a request should use one or the other to specify how the message body should be read.
2. **Front-End Behavior**: The front-end server sees the `Transfer-Encoding: chunked` header and decides to process the request as a chunked request. It reads the chunk size (`0`), sees that there's no data, and forwards what it thinks is the complete request to the back-end server. However, it also forwards the extra data `GPOST / HTTP/1.1 Host: example.com`.
3. **Back-End Behavior**: The back-end server, upon receiving the data, might prioritize the `Content-Length: 6` header over the `Transfer-Encoding: chunked` header. It reads six bytes (`0\r\n\r\nGPO`) as the content of the first request and then interprets the remaining data (`ST / HTTP/1.1 Host: example.com`) as a new, separate HTTP request.
4. **Desynchronization**: At this point, the front-end and back-end servers are desynchronized. They disagree on the boundaries of HTTP requests. The back-end server thinks it has received two separate HTTP requests, while the front-end thinks it has only forwarded one.
5. **Exploitation**: An attacker can exploit this desynchronization to launch various attacks, such as cache poisoning, credential hijacking, and more.
6. **Next Request**: Any subsequent legitimate request from another user might get appended to the remaining data, causing unexpected and potentially harmful behavior.

The attack may look something like this :
```
POST / HTTP/1.1
Host: example.com
Content-Length: 6
Transfer-Encoding: chunked

0

GPOST / HTTP/1.1
Host: example.com
```

If it's the back-end that doesn't support chunked encoding, we'll need to flip the offsets around:
```
POST / HTTP/1.1  
Host: example.com  
Content-Length: 3  
Transfer-Encoding: chunked  
  
6  
PREFIX  
0  
  
POST / HTTP/1.1  
Host: example.com
```

If you are trying to obfuscate your attacks, try using any of these methods:
`Transfer-Encoding: xchunked`
`Transfer-Encoding : chunked`
```
Transfer-Encoding: chunked
Transfer-Encoding: x
```
`Transfer-Encoding:[tab]chunked`
```
GET / HTTP/1.1
Transfer-Encoding: chunked
```
`X: X[\n]Transfer-Encoding: chunked`
```
Transfer-Encoding
 : chunked
```
##### ***Resources***
https://regilero.github.io/english/security/2019/10/17/security_apache_traffic_server_http_smuggling/
https://portswigger.net/research/http-desync-attacks-request-smuggling-reborn
https://book.hacktricks.xyz/pentesting-web/http-request-smuggling