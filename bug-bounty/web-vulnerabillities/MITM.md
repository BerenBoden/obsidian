##### ***Intercepting and spoofing HTTPS traffic***
To both intercept the original request and return a spoofed HTTPS request, an attacker would typically have to carry out a Man-in-the-Middle (MITM) attack, but doing this successfully for HTTPS requires overcoming several significant challenges due to the security features in HTTPS.

Here's how an attacker might theoretically carry it out, step-by-step:

1. **Intercepting the Traffic**: First, the attacker would need to position themselves in a way that they can intercept the victim's web traffic. This is usually done on unsecured Wi-Fi networks or via malware installed on the victim's machine.
2. **SSL Stripping**: One approach to downgrade an HTTPS connection to HTTP, where they can easily manipulate the traffic. However, this method is less effective against websites that implement security features like HSTS (HTTP Strict Transport Security).
3. **Rogue Certificate**: If the attacker wants to spoof HTTPS traffic, they'd need a rogue SSL certificate that the victim's browser would accept. This is difficult because browsers rely on a list of trusted Certificate Authorities (CAs). The attacker would need to compromise a CA or trick the victim into installing a rogue certificate, both of which are non-trivial tasks.
4. **Spoofing and Forwarding**: Once they've set up the MITM attack, they could then intercept the original HTTPS request, create a new request or modify the existing one, and forward it to the actual server. They could also intercept the server's response and modify that before forwarding it to the victim.
5. **Listening In**: Since they're in the middle, they can listen to all the traffic going back and forth between the victim and the server.