### *Set up SSL & HTTPs connections*
To set up Nginx for HTTPS, you'll need to obtain an SSL/TLS certificate and configure Nginx to use it. Here's a step-by-step guide:

**Obtain an SSL/TLS Certificate:**
You can obtain a free certificate from Let's Encrypt using tools like Certbot.
Alternatively, you can purchase a certificate from a certificate authority (CA).

**Install Certbot (for Let's Encrypt):**
Certbot is used to obtain a certificate from Let's Encrypt.
For Ubuntu, use: `sudo apt-get install certbot python3-certbot-nginx`.

**Generate Certificate with Certbot:**
Run Certbot to obtain your certificate and automatically configure Nginx:
`sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com`
Replace `yourdomain.com` with your actual domain name.
Certbot will modify your Nginx configuration to use the new certificate and set up automatic certificate renewal.

> Make sure your current Nginx config is not listening on 443 already before running any of the above commands, or you will run into the error `nginx: [emerg] a duplicate listen 0.0.0.0:443 in /etc/nginx/sites-enabled/berenboden-payload:8`

**Manual Nginx Configuration for SSL/TLS (if not using Certbot):**
Edit the configuration file in `/etc/nginx/sites-available/`, add the following inside the `server` block:
```
listen 443 ssl;
ssl_certificate /path/to/your/fullchain.pem;
ssl_certificate_key /path/to/your/privatekey.pem;

ssl_session_cache shared:le_nginx_SSL:10m;
ssl_session_timeout 1440m;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
```

Replace `/path/to/your/fullchain.pem` and `/path/to/your/privatekey.pem` with the actual paths to your certificate files.

**Redirect HTTP to HTTPS**
To redirect all HTTP traffic to HTTPS, add the following server block in your Nginx configuration:
```
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    return 301 https://$server_name$request_uri;
}
```

**Reload Nginx Configuration:**
After making changes, reload Nginx to apply the new configuration:
`sudo systemctl reload nginx`