### *Manual discovery*
#### Basic enumaration
- /sitemap.xml
- /robots.txt
- Headers
#### Favicon
Find the URL of the favicon on the site, then curl the URL and find the md5sum.
```
curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1406  100  1406    0     0  11645      0 --:--:-- --:--:-- --:--:-- 11619
f276b19aabcb4ae8cda4d22625c6735f  -
```

Find the md5sum and search for a framework that matches on the [OWASP favicon database.](https://wiki.owasp.org/index.php/OWASP_favicon_database)
#### Subdomain enumeration
**SSL issues**
- [ui.ctsearch.entrust.com/ui/ctsearchui](https://ui.ctsearch.entrust.com/ui/ctsearchui)
- [crt.sh](https://crt.sh/)
**Google dorks**
- [Construct dork queries with dorksearch](https://dorksearch.com/)
- -site:www.tryhackme.com  site:*.tryhackme.com
**Tools**
- dnsrecon
- [Sublist3r](https://github.com/aboul3la/Sublist3r)
**Methods**
- Some subdomains aren't always hosted in publically accessible DNS results, such as development versions of a web application or administration portals. Instead, the DNS record could be kept on a private DNS server or recorded on the developer's machines in their /etc/hosts file (or c:\windows\system32\drivers\etc\hosts file for Windows users) which maps domain names to IP addresses.
  
  Scan for virtual hosts. Find the IP address of the server, then you can scan for virutal hosts using the `Host` header: 
  ```
  user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.158.101 -fs 2395
  ```
  
  I used the `-fs 2395` flag because there were too many 200 responses with the response size of 2395 indicating there is nothing of interest with this response.