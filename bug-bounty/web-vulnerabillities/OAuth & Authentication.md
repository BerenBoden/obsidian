### *ChatGPT explaining a simple OAuth flow*
Here are the requests & responses in order.
```
//URL https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/
Request: GET /auth?client_id=aa8vm9jmyoz5qscwuqhn1&redirect_uri=https://0a5a002a037d84f780035315000a000f.web-security-academy.net/oauth-callback&response_type=code&scope=openid%20profile%20email HTTP/2
Host: oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Te: trailers

Response: HTTP/2 302 Found
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Set-Cookie: _interaction=5ovWJrNrUQEU0eP67zj5W; path=/interaction/5ovWJrNrUQEU0eP67zj5W; expires=Wed, 13 Dec 2023 19:35:34 GMT; samesite=lax; secure; httponly
Set-Cookie: _interaction_resume=5ovWJrNrUQEU0eP67zj5W; path=/auth/5ovWJrNrUQEU0eP67zj5W; expires=Wed, 13 Dec 2023 19:35:34 GMT; samesite=lax; secure; httponly
Location: /interaction/5ovWJrNrUQEU0eP67zj5W
Content-Type: text/html; charset=utf-8
Date: Wed, 13 Dec 2023 19:25:34 GMT
Keep-Alive: timeout=5
Content-Length: 99

Redirecting to <a href="/interaction/5ovWJrNrUQEU0eP67zj5W">/interaction/5ovWJrNrUQEU0eP67zj5W</a>.

//URL https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/
Request: GET /interaction/5ovWJrNrUQEU0eP67zj5W HTTP/2
Host: oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
Cookie: _interaction=5ovWJrNrUQEU0eP67zj5W
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Te: trailers

Response: HTTP/2 200 OK
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Content-Type: text/html; charset=utf-8
Etag: W/"d0b-BB/DBy4wzy4H1YEJ3bzyqIygHs4"
Date: Wed, 13 Dec 2023 19:25:35 GMT
Keep-Alive: timeout=5
Content-Length: 4420

//URL https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/
Request: POST /interaction/5ovWJrNrUQEU0eP67zj5W/login HTTP/2
Host: oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
Cookie: _interaction=5ovWJrNrUQEU0eP67zj5W
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/interaction/5ovWJrNrUQEU0eP67zj5W
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Origin: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

username=wiener&password=peter

Response: HTTP/2 302 Found
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Location: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/auth/5ovWJrNrUQEU0eP67zj5W
Date: Wed, 13 Dec 2023 19:25:40 GMT
Keep-Alive: timeout=5
Content-Length: 0

//URL https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/
Request: GET /auth/5ovWJrNrUQEU0eP67zj5W HTTP/2
Host: oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
Cookie: _interaction_resume=5ovWJrNrUQEU0eP67zj5W
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/interaction/5ovWJrNrUQEU0eP67zj5W
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

Response: HTTP/2 302 Found
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Location: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/auth/5ovWJrNrUQEU0eP67zj5W
Date: Wed, 13 Dec 2023 19:25:40 GMT
Keep-Alive: timeout=5
Content-Length: 0

//URL https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/
Request: GET /interaction/5ovWJrNrUQEU0eP67zj5W HTTP/2
Host: oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
Cookie: _interaction=5ovWJrNrUQEU0eP67zj5W; _session=QLB92C9UM9jWXb_1iHgYn; _session.legacy=QLB92C9UM9jWXb_1iHgYn
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/interaction/5ovWJrNrUQEU0eP67zj5W
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

Response: HTTP/2 302 Found
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Set-Cookie: _interaction=5ovWJrNrUQEU0eP67zj5W; path=/interaction/5ovWJrNrUQEU0eP67zj5W; expires=Wed, 13 Dec 2023 19:35:40 GMT; samesite=lax; secure; httponly
Set-Cookie: _interaction_resume=5ovWJrNrUQEU0eP67zj5W; path=/auth/5ovWJrNrUQEU0eP67zj5W; expires=Wed, 13 Dec 2023 19:35:40 GMT; samesite=lax; secure; httponly
Set-Cookie: _session=QLB92C9UM9jWXb_1iHgYn; path=/; expires=Wed, 27 Dec 2023 19:25:40 GMT; samesite=none; secure; httponly
Set-Cookie: _session.legacy=QLB92C9UM9jWXb_1iHgYn; path=/; expires=Wed, 27 Dec 2023 19:25:40 GMT; secure; httponly
Location: /interaction/5ovWJrNrUQEU0eP67zj5W
Content-Type: text/html; charset=utf-8
Date: Wed, 13 Dec 2023 19:25:40 GMT
Keep-Alive: timeout=5
Content-Length: 99

Redirecting to <a href="/interaction/5ovWJrNrUQEU0eP67zj5W">/interaction/5ovWJrNrUQEU0eP67zj5W</a>.

//URL https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/
Request: POST /interaction/5ovWJrNrUQEU0eP67zj5W/confirm HTTP/2
Host: oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
Cookie: _interaction=5ovWJrNrUQEU0eP67zj5W; _session=QLB92C9UM9jWXb_1iHgYn; _session.legacy=QLB92C9UM9jWXb_1iHgYn
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/interaction/5ovWJrNrUQEU0eP67zj5W
Content-Type: application/x-www-form-urlencoded
Content-Length: 0
Origin: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

Response: HTTP/2 302 Found
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Location: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/auth/5ovWJrNrUQEU0eP67zj5W
Date: Wed, 13 Dec 2023 19:25:43 GMT
Keep-Alive: timeout=5
Content-Length: 0

//URL https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/
Request: GET /auth/5ovWJrNrUQEU0eP67zj5W HTTP/2
Host: oauth-0a8b005f03e0848780d4519802200007.oauth-server.net
Cookie: _interaction_resume=5ovWJrNrUQEU0eP67zj5W; _session=QLB92C9UM9jWXb_1iHgYn; _session.legacy=QLB92C9UM9jWXb_1iHgYn
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/interaction/5ovWJrNrUQEU0eP67zj5W
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

Response: HTTP/2 302 Found
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Set-Cookie: _interaction_resume=; path=/auth/5ovWJrNrUQEU0eP67zj5W; expires=Thu, 01 Jan 1970 00:00:00 GMT; samesite=lax; secure; httponly
Set-Cookie: _session=_rnYEtQEw327KO86BDeuD; path=/; expires=Wed, 27 Dec 2023 19:25:43 GMT; samesite=none; secure; httponly
Set-Cookie: _session.legacy=_rnYEtQEw327KO86BDeuD; path=/; expires=Wed, 27 Dec 2023 19:25:43 GMT; secure; httponly
Location: https://0a5a002a037d84f780035315000a000f.web-security-academy.net/oauth-callback?code=tcqDFeP8UxjB2XGytU0dV10ZXO-SiCj4XnFtwwB7WbX
Content-Type: text/html; charset=utf-8
Date: Wed, 13 Dec 2023 19:25:43 GMT
Keep-Alive: timeout=5
Content-Length: 289

//URL https://0a5a002a037d84f780035315000a000f.web-security-academy.net/
Request: GET /oauth-callback?code=tcqDFeP8UxjB2XGytU0dV10ZXO-SiCj4XnFtwwB7WbX HTTP/2
Host: 0a5a002a037d84f780035315000a000f.web-security-academy.net
Cookie: session=0grOlq1IjgshfpBpNtf9SFiYvYiDxMZJ
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
Te: trailers

Response: HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
Set-Cookie: session=WbcgW8HEz5IzicuSmNp5hS2nZsraGazC; Expires=Thu, 14 Dec 2023 19:25:45 UTC; Secure; HttpOnly
X-Frame-Options: SAMEORIGIN
Content-Length: 2948
```
#### 1. Initial Authorization Request
- **Request:** A GET request is made to the `/auth` endpoint to initiate the OAuth process. This includes parameters like `client_id`, `redirect_uri`, and `scope`.
- **Response:** The server responds with a 302 redirect to an `/interaction/:id` endpoint, and sets two cookies: `_interaction` and `_interaction_resume`. These cookies are likely used to track the ongoing authentication/interaction session.
#### 2. User Interaction
- **Request:** The GET request to `/interaction/:id` includes the `_interaction` cookie, which was set in the previous response. This cookie helps the server identify and continue the ongoing interaction session.
- **Response:** The server responds with a 200 OK, indicating the interaction (such as a login form) was successfully served.
#### 3. Login Submission
- **Request:** The POST request to `/interaction/:id/login` includes user credentials (`username` and `password`) and the `_interaction` cookie. This request is for user authentication.
- **Response:** The server responds with a 302 redirect to the `/auth/:id` endpoint. This indicates that the login was likely successful, and the process is moving forward.
#### 4. Authorization Grant
- **Request:** The GET request to `/auth/:id` includes the `_interaction_resume` cookie. This stage is probably where the user consents to the requested access or where the OAuth server finalizes the authorization process.
- **Response:** The server responds with a 302 redirect to the client application's callback URL (`https://0a5a002a.../oauth-callback`) and includes an authorization code in the query parameters. It also sets new `_session` cookies, indicating a new session state.
#### 5. Callback to Client Application
- **Request:** The GET request to `/oauth-callback` on the client application's server includes the `session` cookie and the authorization code.
- **Response:** The client server responds with a 200 OK and sets a new `session` cookie. This step typically involves the client server exchanging the authorization code for an access token with the OAuth server.
#### Summary of Cookie Roles:
- `_interaction` and `_interaction_resume`: Used to maintain state and track the interaction session throughout the OAuth process.
- `_session` and `_session.legacy`: Used to maintain the user session, potentially after successful authentication.
- `session` cookie on the client side: Manages the user's session with the client application, separate from the OAuth interaction.