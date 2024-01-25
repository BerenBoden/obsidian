- Recon methadologies:
    
    - @Mayonaise methods:
    
        This person has an automation flow set up for domain reconnaissance and monitoring, which involves various tools and scripts to manage the process efficiently. The main components of this flow are as follows:
        
        1. Diffing: They use a text file containing a list of domains and a bash script to perform diffs between different domain lists.
        2. Mass DNS and Amass: They use mass DNS to resolve domain names and Amass for passive domain enumeration.
        3. Continuous monitoring: The system is designed to run continuously, with different "workers" or shell scripts waiting for new files containing domain lists.
        4. Chunking data: The process is designed to break down the data into smaller bits, allowing the user to be constantly fed with new information.
        5. Custom brute-forcing: To discover new domains, they use their own custom script based on patterns or "recipes" derived from a deep analysis of existing domains. This enables them to find new domains more efficiently than just using Amass's built-in brute-forcing feature.
        
        Overall, the person's automation flow is focused on domain reconnaissance, monitoring, and discovery. It uses various tools, such as bash scripts, Mass DNS, and Amass, as well as custom scripts for brute-forcing new domains. The system is designed to run continuously, processing new data as it becomes available, and breaking down the data into smaller chunks for easier analysis.
        
    - @YassineAboukir methods:
        
        This person describes their automated recon methodology as follows:
        
        Subdomain Enumeration: They use Amass in passive mode and combine it with various APIs (e.g. SecurityTrails) to enumerate subdomains. They don't pay for APIs since the free ones suffice for their needs.
        
        Permutation: They perform permutations on the discovered subdomains using a custom wordlist containing devops and modern software keywords. They mention a Rust-based tool called "repgen" that performs permutations very quickly.
        
        DNS Resolution: They use PureDNS to resolve subdomains and discard non-resolving ones.
        
        DNS Enumeration: They use dnsx to enumerate DNS records, mainly focusing on A records.
        
        Filtering: They filter out IPs pointing to CDNs (e.g. Cloudflare, CloudFront, Akamai) as they might return false information.
        
        Port Scanning: They use nmap to scan all 65,000+ ports for each IP address, preferring it over other tools like masscan due to its reliability and fewer false positives.
        
        HTTP Metadata: They use httpx (from Project Discovery) to collect HTTP metadata such as titles, status, locations, and technologies.
        
        Vulnerability Scanning: They use nuclei to scan for vulnerabilities using public and custom templates.
        
        Content Discovery: They perform content discovery separately, using tools like ffuf for directory brute-forcing, and a wordlist called "One List For All" by so_two_days_six. They also use a Go tool by Corben called "get-all-urls" to find endpoints.
        
        Burp Suite: They use Burp Suite's crawler to crawl the subdomains and extract JavaScript files. They then use Link Finder and nuclei templates to find endpoints and hardcoded API endpoints in those JS files.
        
        Throughout the process, they store all the information in a PostgreSQL database. They scan all the ports and manually decide which subdomains to test based on interesting titles or DNS records.
        
- Recon vidoes:
    
    - [https://www.youtube.com/watch?v=DVdB0Z18HCA&list=PLKAaMVNxvLmAkqBkzFaOxqs3L66z2n8LA&index=64](https://www.youtube.com/watch?v=DVdB0Z18HCA&list=PLKAaMVNxvLmAkqBkzFaOxqs3L66z2n8LA&index=64) @ @ 1:08:00
    - [https://www.youtube.com/watch?v=FU1fWMwGMuY](https://www.youtube.com/watch?v=FU1fWMwGMuY) @ 49:50
    

### **Notes from The Bug Hunters Methodology - Application Analysis v1**

This post will consist of notes taken from [The Bug Hunter’s Methodology: Application Analysis v1](https://www.youtube.com/watch?v=HmDY7w8AbR4) - a talk by Jason Haddix at Nahamcon 2022. These notes are mostly for my own future review, but hopefully other people will find it useful as well.

Many people have been teaching how to inject an XSS payload, but not how to systematically find vulnerabilities in the first place. Jason has created an AppSec edition of his methodology when it became large enough to be split into recon and AppSec parts.

The following books are recommended:

- [The Web Application Hacker’s Handbook 2](https://www.amazon.com/Web-Application-Hackers-Handbook-Exploiting/dp/1118026470/ref=sr_1_1?crid=223GVK9BGXP70&keywords=web+application+hackers+handbook+2&qid=1655126465&sprefix=web+application+hackers+handbook+2%2Caps%2C186&sr=8-1) - read this at least twice!
- [Real World Bug Hunting](https://www.amazon.com/Real-World-Bug-Hunting-Field-Hacking/dp/1593278616/ref=sr_1_1?crid=3MSIL2826QZGD&keywords=Real+World+bug+hunting&qid=1655126510&sprefix=real+world+bug+hunting%2Caps%2C160&sr=8-1)
- [OWASP Web Security Testing Guide](https://github.com/OWASP/wstg)
- [Bug Bounty Bootcamp](https://www.amazon.com/Bug-Bounty-Bootcamp-Reporting-Vulnerabilities/dp/1718501544)
- [The Hacker’s Playbook 3](https://www.amazon.com/Hacker-Playbook-Practical-Penetration-Testing/dp/1980901759/ref=sr_1_1?crid=IPJY7MVTIQO8&keywords=the+hackers+playbook+3&qid=1655126695&s=books&sprefix=the+hackers+playbook+3%2Cstripbooks%2C156&sr=1-1)
- [Breaking into information security](https://www.amazon.com/Breaking-into-Information-Security-Crafting-ebook/dp/B019K7CMMW/ref=sr_1_1?crid=AOOXS637SAU4&keywords=breaking+into+information+security&qid=1655126724&s=books&sprefix=breaking+into+information%2Cstripbooks%2C167&sr=1-1)
- [Hands on Hacking](https://www.amazon.com/Hands-Hacking-Matthew-Hickey/dp/1119561450/ref=sr_1_1?crid=W4KBT61E7RQ5&keywords=Hands+on+hacking&qid=1655126763&s=books&sprefix=hands+on+hacking%2Cstripbooks%2C159&sr=1-1)
- [Bug Bounty Playbook](https://payhip.com/b/wAoh) - similar to Jason’s methodology.

The following resources are recommended for practice:

- [PentesterLab](https://pentesterlab.com/) - paid
- [Web Security Academy](https://portswigger.net/web-security) - paid
- [Hack The Box](https://www.hackthebox.com/) - paid
- [Vulnhub](https://www.vulnhub.com/) - free
- [OWASP Vulnerable Web Application Directory](https://owasp.org/www-project-vulnerable-web-applications-directory/) - free

The free ones also are good to practice some sysadmin stuff, as you’re supposed to know that as well for bounty hunting.

Recommended security content creators to follow:

- @danielmiessler [Twitter](https://twitter.com/danielmiessler) [Podcast](https://danielmiessler.com/podcast/)
- @stok [Twitter](https://twitter.com/stokfredrik) [Youtube](https://www.youtube.com/c/STOKfredrik)
- @brutelogic [Twitter](https://twitter.com/brutelogic) [Blog](https://brutelogic.com.br/blog/)
- @InsiderPhD [Twitter](https://twitter.com/InsiderPhD) [Youtube](https://www.youtube.com/c/InsiderPhD)
- @infosec_au [Twitter](https://twitter.com/infosec_au) [Blog](https://blog.assetnote.io/)
- @Farah_Hawaa [Twitter](https://twitter.com/Farah_Hawaa) [Youtube](https://www.youtube.com/c/farahhawa)
- @zseano [Twitter](https://twitter.com/zseano) [Youtube](https://www.youtube.com/c/zseano)
- @hacker_ [Twitter](https://twitter.com/hacker_) [Blog](https://corben.io/)
- @hakluke [Twitter](https://twitter.com/hakluke) [Blog](https://hakluke.com/blog/)
- @albinowax [Twitter](https://twitter.com/albinowax) [Blog](https://skeletonscribe.net/)
- @tomnomnom [Twitter](https://twitter.com/tomnomnom) [Youtube](https://www.youtube.com/user/TomNomNomDotCom/featured)
- @_JohnHammond [Twitter](https://twitter.com/_JohnHammond) [Youtube](https://www.youtube.com/johnhammond010)
- @ippsec [Twitter](https://twitter.com/ippsec) [Youtube](https://www.youtube.com/c/ippsec)
- @nahamsec [Twitter](https://twitter.com/nahamsec) [Youtube](https://www.youtube.com/c/nahamsec)

Generally there’s a lot of bounty hunting community on Twitter.

Don’t be intimidated by bug bounty programs by high profile companies as they have a lot of systems online that are bound to have things you can find. More complexity and scale - more opportunity for bugs. The same applies to bounty programs that involve pre-testing before they are publicly available, open source projects and paid software products. There are bugs in every application.

Don’t merely touch the surface of the app - most of the good stuff is to be found beyond the authentication.

There are following layers to security testing:

- Open ports and services
- Web hosting software
- Application framework
- Application: custom code
- Application libraries
- Integrations

We need to do technology profiling first. The following browser extensions can be used to find what technologies site is based on:

- [Whatruns](https://www.whatruns.com/)
- [Wappalyzer](https://www.wappalyzer.com/)

There is also [webalyze](https://github.com/rverton/webanalyze) - a CLI tool that can be integrated into automated scanning pipelines.

Now it’s time to check that no known vulns, default credentials, framework login pages and the like are present on the site (related to non-custom code). This can be done with Nuclei or Nessus. Nuclei has plenty of checks for CVEs, admin panels, information leaks and the like. There’s other tools for this:

- [Gofingerprint](https://github.com/Static-Flow/gofingerprint)
- [Sn1per](https://github.com/1N3/Sn1per)
- [Intrigue Core](https://core.intrigue.io/)
- [Vulners](https://vulners.com/plugins#burp) (Burp extension)
- [Jaeles](https://github.com/jaeles-project/jaeles)
- [retire.js](https://github.com/retirejs/retire.js/)

However, overreliance on Nuclei can be a competitive disadvantage if it is being used to scan what everyone else is scanning - you are merely going to find dupes. But you can still find vulnerabilities if you use Nuclei for fresh or obscure targets. Nuclei templates are very easy to write which enables you to start using it to find newly published vulnerabilities.

You also want to do some port scanning. Jason recommends [naabu](https://github.com/projectdiscovery/naabu) by Project Discovery. It can be integrated with Nmap for service scanning.

Next phase is content discovery. This is very important part that has several sections:

- Tech-based discovery
- COTS / PAID / OSS
- Custom
- Historical
- Recursive
- Mobile APIs
- Change Detection

The following tools are recommended for content discovery:

- [Turbo Intruder](https://portswigger.net/bappstore/9abaa233088242e8be252cd4ff534988) (Burp extension)
- [Gobuster](https://github.com/OJ/gobuster)
- [ffuf](https://github.com/ffuf/ffuf)
- [Feroxbuster](https://github.com/epi052/feroxbuster)
- [dirsearch](https://github.com/maurosoria/dirsearch)
- [Wfuzz](https://github.com/xmendez/wfuzz)

These will be used to bruteforce/fuzz directories on web servers to discover stuff. Gobuster can also find subdomains and S3 buckets.

It is also important to have pre-made lists for content discovery to look for common things likes swagger.json file and various web-server or framework specific directories. Sometimes this lets us find unprotected configuration files with database credentials and other sensitive information. There are some lists available that are based on e.g. scraping robots.txt files from most popular sites and compiling a big list of things that sites don’t want bots to acesss/index. Some lists are technology-specific, i.e. to be used against PHP or Java sites, some are generic. Make sure to you an up-to-date list. See: [https://wordlists.assetnote.io/](https://wordlists.assetnote.io/)

If an application is built on open source stuff you can use [Source2URL](https://github.com/danielmiessler/Source2URL/blob/master/Source2URL) - a Bash script that goes through source code, harvests URLs and tries to access them through proxy (e.g. Burp).

But what if it’s a paid that that is being deployed (e.g. some CRM solution)? In that case you may be able to get access by contacting the paid software vendor and get a demo access by pretending to be interested in buying it. This will enable you to explore the paid software and note down various API endpoints and other URL paths to make your own wordlist for content discovery. This custom list would be then used against an actual target.

Custom content discovery lists can be also built by spidering sites and using Scavenger in Burp to make the list for you.

Content discovery lists can also be built from historical data. [gau](https://github.com/lc/gau) is a tool that will access Open Threat Exchange, Wayback Machine and CommonCrawl for a list of URLs to all known historical pages on the site. Output of this tool can be fed into [wordlistgen](https://github.com/ameenmaali/wordlistgen) to make a wordlist based on list of URLs. For some sites gau will yield a lot of duplicate entries that can be cleaned up with [trashcompactor](https://github.com/michael1026/trashcompactor).

Recursive bruteforcing should be used on URL paths that yield HTTP 401 response, as if you go deep enough you may find some level that does not enforce authentication due to misconfiguration and then you’re in. Jason mentions finding a massive amount of unprotected private data and access to SMS panel by doing recursive content discovery.

Many times same domain is used for mobile API as well, which means that mobile API endpoints are covered by bug bounty program. They can be discovered by parsing APK file with tool like [APKLeaks](https://github.com/dwisiswant0/apkleaks). This will also look for hardcoded API keys and other things that may be useful for further analysis.

Another tip is to monitor for website changes by using tools like [ChangeDetection](https://github.com/dgtlmoon/changedetection.io). Another ways to find about changes is signing up for affiliate program, watching the conference talks by company that develops the app in question, reading their email newsletter. In bounty hunting it is of importance to be the first to find new vulnerabilities ASAP after the code has changed.

Next stage is application analysis.

There are 6 highly important questions to ask when it comes to app analysis.

1. How does it pass data? Is it by resource and URL parameters or is it a combination of HTTP method and URL path? Not getting this right may derail your further fuzzing/scanning efforts.
2. Where and how does it reference users? How are they identified (UUID, user ID, email, something else?) and where is the identifier communicated? Does it enforce consistency between session and user?
3. Does it have multi-tenancy or multi-user functionality? This would have potential for access, authentication and authorization bugs as it’s very hard for developers to enforce boundaries properly in all cases. This can yield IDORs, information disclosure bugs and so forth. Are there different levels of permissions within the app? Are there ways for lower level user to view information that only admin is supposed to access?
4. Does the site have unique threat model? For example, securing stream keys and private streamer information is very important for Twitch, as leaking this data may lead to all kinds of trouble. This also applies to medical sites.
5. Has there been prominent examples of vulnerabilities and attacks against the system being analysed? Just Google for this.
6. How does the app and/or framework handle XSS, CSRF and injection attacks? What are countermeasures like and how can they be bypassed?

Spider the site to ensure coverage with something like Burp or ZAP. You need to cover as many code paths as possible. Alternatively, you can use [hakrawler](https://github.com/hakluke/hakrawler) or [GoSpider](https://github.com/jaeles-project/gospider) for GUI-less automation.

You may also want to parse JavaScript code to find sensitive API endpoints and maybe even API keys with tool like [xnLinkFinder](https://github.com/xnl-h4ck3r/xnLinkFinder). There’s also Burp extension for that.

Sometimes JS code will be minified or obfuscated, but will still contain things to be find there.

Sometimes old JS libraries will be used and will contain exploitable vulnerabilities. This can be found with retire.js.

Heat mapping is an idea that there are certain places in the app that are of particular interest or potential for bugs: upload functions, integrations with 3rd parties, external data storage (e.g. in S3 bucket), multipart forms, account sections, APIs and error pages.

Few years ago there was research project (HUNT) to determine what URL parameters tend to statistically correlate with specific types of vulnerabilities. This provides a good idea on what to check first and prioritise parameters/routes in terms of their potential for bugs. There’s a [Burp extension](https://github.com/bugcrowd/HUNT) that helps with this.

### Recon

- Methods for reconnaissance of website information: DNS brute forcing: This technique involves querying DNS servers for all possible subdomains of a target domain using a wordlist of common subdomain names, such as "admin", "test", or "dev". There are several DNS brute forcing tools available, including Sublist3r, Amass, and Knockpy.
- Certificate Transparency logs: Certificate Transparency logs are public records of SSL/TLS certificates that have been issued by trusted Certificate Authorities. By searching these logs for certificates issued to a target domain, researchers can discover subdomains that are using SSL/TLS encryption. Tools like CT-Exposer and Censys can automate this process.
- Public search engines: Search engines like Google, Bing, and Shodan can be used to discover subdomains of a target domain by searching for specific keywords or phrases associated with the target. For example, searching for "site:targetdomain.com" can reveal subdomains that are publicly accessible and indexed by search engines.
- Passive DNS: Passive DNS databases like Farsight and VirusTotal can be used to discover subdomains that have been previously observed by these databases. By searching for DNS resolutions associated with a target domain, researchers can discover subdomains that have been previously used.
- Brute forcing with web server headers: This technique involves brute forcing subdomains using HTTP response headers like "Server" or "X-Powered-By". By querying a target domain for all possible subdomains and analysing the web server headers in the HTTP response, researchers can discover subdomains that are using the same web server software as the main domain.
- Fuzzing: Fuzzing is a technique used to discover vulnerabilities or unexpected behaviour in software by sending it malformed or unexpected input. This can include sending invalid characters, excessively long inputs, or inputs designed to trigger specific error conditions. By observing how the software responds to these inputs, researchers can identify potential vulnerabilities that could be exploited by attackers.
- Brute forcing: Brute forcing is a technique used to discover passwords or other secrets by guessing them systematically. This involves trying many possible combinations of characters until the correct one is found. Brute forcing can be effective against weak passwords or passwords that have been leaked in data breaches.
- Generating permutations: Generating permutations involves creating variations of known values or patterns to try to discover new values. For example, if a known subdomain is "[admin.example.com](http://admin.example.com/)", generating permutations could involve trying variations like "[dev.example.com](http://dev.example.com/)", "[test.example.com](http://test.example.com/)", or "[staging.example.com](http://staging.example.com/)" to discover additional subdomains that may be in use.

### Tools

1. Reconnaissance tools: These tools gather information about the target application, its infrastructure, and related technologies. Examples include Nmap, Shodan, and Google Dorks.
2. Vulnerability scanners: These tools automate the process of discovering potential vulnerabilities in web applications. Examples include Nikto, Acunetix, and OWASP ZAP.
3. Proxy tools and intercepting proxies: These tools allow you to intercept, inspect, and modify HTTP/HTTPS requests and responses, helping you understand how the application works and identify vulnerabilities. Examples include Burp Suite, OWASP ZAP, and Fiddler.
4. Web application fuzzers: These tools send a series of unexpected inputs to the target application to trigger errors, crashes, or other unexpected behaviors that may reveal vulnerabilities. Examples include wfuzz, Boofuzz, and JBroFuzz.
5. Exploitation tools: These tools help security researchers exploit identified vulnerabilities to gain unauthorized access or perform other malicious actions. Examples include Metasploit, sqlmap, and XSSer.
6. Post-exploitation tools: These tools are used after successful exploitation to maintain access, escalate privileges, or pivot to other systems. Examples include Mimikatz, Empire, and PowerSploit.
7. Static and dynamic code analysis tools: These tools analyze the source code or the runtime behavior of the application to identify vulnerabilities or coding errors. Examples include SonarQube, FindBugs, and Veracode.
8. Password cracking and authentication testing tools: These tools are used to test the strength of passwords and authentication mechanisms. Examples include John the Ripper, Hydra, and Hashcat.
9. Content discovery and directory brute-forcing tools: These tools help discover hidden directories, files, or API endpoints in web applications. Examples include DirBuster, gobuster, and ffuf.
10. SSL/TLS analysis tools: These tools test the security of SSL/TLS configurations and identify potential weaknesses. Examples include SSLyze, [testssl.sh](http://testssl.sh), and Qualys SSL Labs.
11. Web application firewalls (WAF) and evasion tools: These tools are used to test the effectiveness of WAFs and bypass security mechanisms. Examples include ModSecurity, wafw00f, and SQLMap Tamper Scripts.
12. Traffic analysis and network monitoring tools: These tools monitor network traffic and help identify suspicious activity or potential vulnerabilities. Examples include Wireshark, tcpdump, and Security Onion.

- Main... burpsuite pro: git clone [https://github.com/SNGWN/Burp-Suite](https://github.com/SNGWN/Burp-Suite) && cd Burp-Suite && ./Kali_Linux_Setup.sh go: sudo apt install golang-go docker: sudo apt install -y [docker.io](http://docker.io/) && sudo systemctl enable docker --now && docker && sudo usermod -aG docker $USER
    
- FireFox extensions... wappalyzer react-dev-tools foxy proxy open multiple urls
    
- Recon... kiterunner: git clone [https://github.com/assetnote/kiterunner](https://github.com/assetnote/kiterunner) && cd kiterunner && make build gobuster: [https://github.com/OJ/gobuster.git](https://github.com/OJ/gobuster.git) || sudo apt install gobuster ffuf: [https://github.com/ffuf/ffuf.git](https://github.com/ffuf/ffuf.git) proxychains4: sudo apt install proxychains4 mubeng: [https://github.com/kitabisa/mubeng.git](https://github.com/kitabisa/mubeng.git) amass: [https://github.com/OWASP/Amass.git](https://github.com/OWASP/Amass.git) sublist3r: [https://github.com/aboul3la/Sublist3r](https://github.com/aboul3la/Sublist3r) subfinder: waybackurls:[https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls) httprobe: [https://github.com/tomnomnom/httprobe](https://github.com/tomnomnom/httprobe) || sudo apt install httprobe dirb: wpscan: [https://github.com/wpscanteam/wpscan.git](https://github.com/wpscanteam/wpscan.git)
    
- Post exploitation... SILENTTRINITY: [https://github.com/byt3bl33d3r/SILENTTRINITY](https://github.com/byt3bl33d3r/SILENTTRINITY) msf: docker pull metasploitframework/metasploit-framework
    
- Websites... technology behind site: [](https://w3techs.com/)[https://w3techs.com](https://w3techs.com)
    
- **Mubeng proxy rotation:**
    
    ```jsx
    mubeng -a 127.0.0.1:8069 -f ~/bounty/tools/proxies.txt -r 1 -m random
    ```
- Brute force ssh.
    
    ```jsx
    hydra -l john -P ~/bounty/wordlists/SecLists/Passwords/2020-200_most_used_passwords.txt -t 4 ssh://10.10.150.235
    ```
