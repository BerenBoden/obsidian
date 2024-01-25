1. Company recon
    
    - Collect & monitor company acquisition data from Crunchbase [https://www.crunchbase.com/](https://www.crunchbase.com/).
        
    - Use Webanalyze to check technology stack.
        
        ```jsx
        webanalyze -host <https://example.com> -crawl 2 -o ~/bounty/info/tech.txt
        ```
        
    - Utilize Github to find sensitive data related to the target domain.
        
        ```jsx
        python3 github-search.py -q 'example.com' -t <YOUR_GITHUB_TOKEN> > ~/bounty/github/github-search.txt
        ```
        
    - Download any Github repositories and grep through results.
        
        - repositories.txt should contain the list of target repositories, one per line.
            
            ```jsx
            while read p; do
            git clone <https://www.github.com/USERNAME/$p>
            done <repositories.txt
            ```
            
        - Run.
            
            ```jsx
            egrep -Ri -f regex-patterns.txt
            ```
            
        - Loop through directories for secrets.
            
            ```jsx
            #!/bin/bash
            # Read the list of repositories from a file
            while read repo_path; do
              # Change directory to the repository
              cd "$repo_path"
              
              # Loop through each pattern in the patterns.txt file
              while read pattern; do
                # Search for the pattern in the commit history
                git rev-list --all | xargs -I{} git grep -i -E "$pattern" {}
            
                # You can also add a separator between results for better readability
                echo "-----------------------------------"
              done < /path/to/patterns.txt
            done < /path/to/list_repos.txt
            ```
            
    - Google dork automation with custom script [https://github.com/BerenBoden/auto-dorker**.**](https://github.com/BerenBoden/auto-dorker**.**)
        
    - Obtain ASN data using [bgp.he.net](http://bgp.he.net).
        
        ```jsx
        ~/bounty/info/asn.txt
        ```
        
    - Use OWASP Amass to get more information about the target domain.
        
        - Gather general information
            
            ```jsx
            amass intel -org example.com -o ~/bounty/info/intel.txt
            ```
            
        - Run a loop over asn’s collected from metagbigor or [bgp.he.net](http://bgp.he.net)
            
            ```bash
            for i in $(cat ~/bounty/info/asn.txt); do echo""; echo "ASN $i";echo ""; amass intel -active -asn $i;echo ""; done
            ```
            
            > Sometimes amass will hang on an ASN number. Go to htop and kill the process in which the ASN number is hanging on, and continue from where the loop stopped.
            
    - Perform Reverse WHOIS using DomLink.
        
        ```jsx
        python3 domLink.py -D example.com -o ~/bounty/info/whois.txt
        ```
        
    - Use Shodan to search for the target's IP addresses, open ports, services, and technologies in use.
        
        ```jsx
        shodan search --fields ip_str,port,org,hostnames,transport,product,version "hostname:example.com"
        ```
        
    - Consider using additional tools like Censys, BinaryEdge, or ZoomEye for further asset discovery and technology profiling.
        
    - Investigate the gathered information for potential vulnerabilities, misconfigurations, or outdated software.
        
2. Subdomain enumeration. Mass port scanning/probing. Collecting headers, bodies and response data.
    
    - Perform passive domain enumeration.
        
        - passive-enum.txt. Interlace list of all passive domain enumeration tools.
            
            ```
            subfinder -d _target_ -silent -o _output_/subfinder.txt
            python3 ~/bounty/tools/Sublist3r/sublist3r.py -d _target_ -o _output_/sublist3r.txt
            amass enum -passive -d _target_ -o _output_/amass.txt
            subdomainizer -u https://_target_ -o _output_/subdomainizer.txt
            crtsh-ls --format='${{padlen .NameValue 30}}' _target_ >> _output_/crtsh-ls.txt
            assetfinder --subs-only _target_ >> _output_/assetfinder.txt
            ```
            
        - [enum.sh](http://enum.sh). Shell script to automate putting each list from each tool into a separate file, and combining domains with anew to ensure no duplicates.
            
            ```bash
            #!/bin/bash
            combined_domains=~/bounty/domains/$1-enum/combined-$1-domains.txt
            scan_lists=~/bounty/domains/$1-enum
            scan_scripts=~/bounty/domains/$1-enum.txt
            all_domains=~/bounty/domains/all-domains.txt
            
            interlace -tL seed-domains.txt -cL "$scan_scripts" -o "$scan_lists" -threads 1
            echo "Interlace finished"
            
            if [ ! -f "$combined_domains" ]; then
                touch "$combined_domains"
            fi
            for file in "$scan_lists"/*.txt; do
                cat "${file}" | anew "$combined_domains"
            done
            echo "Combining domains finished"
            
            if [ ! -f $all_domains ]; then
                touch $all_domains
            fi
            
            cat "$combined_domains" | sort -u | anew $all_domains
            ```
            
            > `sudo chmod +x enum.sh ./enum.sh (passive, active)`
            
        - Use host command to find hosts of domains
            
            ```jsx
            cat ~/bounty/domains/alive-domains.txt | xargs -I{} host {} | tee -a ~/bounty/domains/host-domains.txt
            ```
            
        
        need to find way to cut the new hosts found.
        
        - Combine all domains.
            
            ```jsx
            #!/bin/bash
            active_domains=~/bounty/domains/active-enum/combined-active-domains.txt
            passive_domains=~/bounty/domains/passive-enum/combined-passive-domains.txt
            active_rotation_domains=~/bounty/domains/active-enum/combined-active-rotation-domains.txt
            wayback_domains=~/bounty/domains/active-enum/wayback-active-domains.txt
            all_domains=~/bounty/domains/all-domains.txt
            host_domains=~/bounty/domains/host-domains.txt
            
            if [ ! -f $all_domains ]; then
                touch $all_domains
            fi
            
            cat $active_domains $wayback_domains $passive_domains $active_rotation_domains $host_domains | sort -u | anew $all_domains
            ```
            
            > `sudo chmod +x combine-domains.sh ./combine-domains.sh`
            
        - Use httprobe to enumerate all-domains.txt to find live domains.
            
            ```jsx
            cat all-domains.txt | httprobe | anew ~/bounty/domains/alive-domains.txt
            ```
            
        - [waybackurls.sh](http://waybackurls.sh). Enumerate alive-domains.txt to find older domains with waybackurls.
            
            ```
            #!/bin/bash
            wayback_domains=~/bounty/domains/wayback-domains.txt
            alive_domains=~/bounty/domains/alive-domains.txt
            
            if [ ! -f "$wayback_domains" ]; then
                touch "$wayback_domains"
            fi
            
            cat $alive_domains | waybackurls | anew $wayback_domains
            echo "Enumerating waybackurls finished."
            ```
            
            > `sudo chmod +x waybackurls.sh ./waybackurls.sh`
            
        - Run [combine-domains.sh](http://combine-domains.sh)
            
            > cat ~/bounty/domains/wayback-domains.txt | anew ~/bounty/domains/all-domains.txt | httprobe | anew ~/bounty/domains/alive-domains.txt
            
    - Utilize Fairly Fast File manager to send requests to all live domains, gather header and body information.
        
        ```bash
        cat ~/bounty/domains/alive-domains.txt | fff -d 3 -S -o roots
        ```
        
        - Find all related .js files in the .body files.
            
            ```bash
            find ~/bounty/domains/roots -type f -name "*.body" | xargs grep -Eo '(http|https)://[^/"].*?\\.js'
            ```
            
        - Get title information from all .body files in roots.
            
            ```bash
            find ~/bounty/domains/roots -type f -name *.body | html-tool tags title | nano -
            ```
            
        - Use gf to search for servers.
            
            ```bash
            gf servers ~/bounty/domains/headers.txt -o ~/bounty/domains/servers.txt
            ```
            
        - Send all headers to headers.txt.
            
            ```bash
            gf meg-headers ~/bounty/domains/roots | sort -u | anew ~/bounty/domains/headers.txt
            ```
            
        - Use grep -Hnri to search for interesting headers.
            
            ```bash
            grep -Hnri '403 Forbidden’ ~/bounty/domains/roots
            ```
            
    - (maybe)Use httpx for mass scanning
        
        ```jsx
        cat all-domains.txt | httpx -title -wc -sc -cl -ct -web-server -asn -o ~/bounty/domains/httpx-domains.txt -p 8080,8000,8443,443,8008,3000,5000,7070,9200,15672,9000 -threads 10 -location
        ```
        
    - Amazon S3.
        
        - Find all AmazonS3 buckets in relation to a domain name
            
            ```bash
            find ~/bounty/domains/roots -type f -name "*.headers" -exec grep -l "Server: AmazonS3" {} \\; | awk -F'/' '{print $(NF-1)}’
            ```
            
        - Loop through output of domains related to an S3 bucket, and use dig to find the CNAME
            
            ```bash
            find ~/bounty/domains/roots -type f -name "*.headers" -exec grep -l "Server: AmazonS3" {} \\; | awk -F'/' '{print $(NF-1)}' | while read -r domain; do dig "$domain" CNAME +short; done
            ```
            
    - Use whoxy for reversewhois
        
3. Subdomain/API brute-forcing
    
    - Use FFUF to brute force subdomains with a wordlist based upon the technology and information gathered in recon faze.
        
        ```jsx
        ffuf -w wordlist.txt -u <https://FUZZ.example.com> -mc 200,301,302,403,404
        ```
        
    - If an API endpoint has been found, use kiterunner to scan the target.
        
        ```jsx
        kr scan targets.txt -A=apiroutes-210228 -x 10 --ignore-length=34 --user-agent ""
        ```
        
        ```jsx
        kr scan targets.txt -A=apiroutes-210228:10000 -x 5 --ignore-length=34 -t 1
        ```
        
        ```jsx
        kr scan targets.txt -A=apiroutes-210228:5000 -x 1 --ignore-length=34 --rate  -t 3
        ```
        
4. Alteration scanning
    
    - Run OWASP Amass for active and passive subdomain enumeration, as well as alteration scanning
        - `amass enum -d example.com -o output.txt`
5. CVE scanning
    
    - Scan for known vulnerabilities, default credentials, and framework login pages using Nuclei, Nessus, or other tools like Gofingerprint, Sn1per, Intrigue Core, Vulners, Jaeles, and retire.js.
6. Port analysis
    
    - Conduct a mass port scan using Masscan
        - `masscan -p1-65535 --rate 1000 -iL target_ips.txt -oL output.txt`
    - Run naabu
        - `naabu -host example.com`
    - Use Dnmasscan to run Masscan followed by Nmap for detailed service enumeration
        - `python3 dnmasscan.py target_ips.txt output.txt --rate 1000 --top-ports 1000`
    - Brute force credentials on discovered services using Brutespray
        - `brutespray --file nmap.gnmap`
    - For port scanning, consider using naabu by Project Discovery and integrate it with Nmap for service scanning.
7. Content discovery
    
    - Utilize tools like Turbo Intruder, Gobuster, ffuf, Feroxbuster, dirsearch, and Wfuzz for content discovery.
    - Use pre-made and custom wordlists, historical data, and technology-specific lists for content discovery.
    - Leverage Source2URL for open source projects, and consider obtaining demo access for paid software to build custom lists.
    - Perform recursive content discovery and mobile API endpoint discovery when applicable.
8. Domain flyovers
    
    - Capture screenshots of discovered web services using HTTP screenshot
        - `httpscreenshot -iL target_urls.txt -o output_folder`
9. Domain takeovers
    
    - Check for potential subdomain takeover vulnerabilities using the can-I-take-over-xyz list (**[https://github.com/EdOverflow/can-i-take-over-xyz**](https://github.com/EdOverflow/can-i-take-over-xyz**)).
    - Utilize tools like SubOver, Subjack, or Tko-subs to automate the process of identifying subdomain takeover opportunities.
    - Investigate expired or misconfigured DNS records, as well as orphaned cloud resources for potential takeover vulnerabilities.
    - Validate any findings manually and verify the presence of a takeover vulnerability before attempting to claim the subdomain.
10. SSL/TLS analysis
    
    - Use tools like SSLyze, [testssl.sh](http://testssl.sh), or SSLScan to analyze the SSL/TLS configuration of the target web services.
    - Check for weak ciphers, protocol vulnerabilities, and expired or self-signed certificates.
    - Investigate certificate transparency logs using tools like [crt.sh](http://crt.sh) or Google's Certificate Transparency Search to discover additional subdomains or misconfigurations.
11. API security testing
    
    - Identify API endpoints by inspecting the target's JavaScript files, mobile applications, and documentation.
    - Use tools like Postman, Insomnia, or Burp Suite to test API endpoints for security vulnerabilities.
    - Leverage API-specific testing tools like ZAP API Scan, Restler, and Fuzzapi to automate the testing process.
    - Test for common API vulnerabilities such as authentication bypass, rate limiting issues, insecure direct object references, and data leakage.
12. Automated vulnerability scanning
    
    - Perform automated vulnerability scanning using Nuclei
        - `nuclei -l target_urls.txt -t nuclei-templates/ -o output.txt`
    - Consider using other vulnerability scanners like OpenVAS, Nexpose, Acunetix, or Burp Suite Scanner to identify additional vulnerabilities.
    - Keep in mind that automated scanners have limitations and may produce false positives, so it's important to manually validate findings.
13. Manual security testing
    
    - Conduct manual security testing using tools like Burp Suite, OWASP ZAP, or Fiddler to identify vulnerabilities that automated scanners might miss.
    - Investigate the target's authentication mechanisms, access controls, input validation, output encoding, and other security measures.
    - Leverage specialized tools like SQLMap, NoSQLMap, XSStrike, and Commix for testing specific vulnerabilities such as SQL injection, XSS, and command injection.
    - Remember to follow a structured methodology like OWASP Testing Guide, OWASP Application Security Verification Standard (ASVS), or the Penetration Testing Execution Standard (PTES) during the manual testing process.

## **Recon tools:**

**Tools required**

ASN Data Automation: `go install github.com/j3ssie/metabigor@latest`

OWASP Amass: `go install -v github.com/owasp-amass/amass/v3/...@master`

DomLink: `git clone <https://github.com/vysecurity/DomLink.git> && cd DomLink && pip install -r requirements.txt`

SubDomainizer: `git clone <https://github.com/nsonaniya2010/SubDomainizer.git> && cd SubDomainizer && pip3 install -r requirements.txt`

Crtsh-ls: `wget <https://github.com/koshatul/crtsh-ls/releases/download/v0.3.0/crtsh-ls-v0.3.0-linux-amd64.zip> && unzip crtsh-ls-v0.3.0-linux-amd64.zip`

Metabigor: `go install github.com/j3ssie/metabigor@latest`

Webanalyze: `go install -v github.com/rverton/webanalyze/cmd/webanalyze@latest`

Hakrawler: `go install github.com/hakluke/hakrawler@latest`

SubFinder: `go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest`

Github-search: `go install github.com/gwen001/github-subdomains@latest`

Github-secrets: [https://github.com/eth0izzle/shhgit/](https://github.com/eth0izzle/shhgit/)

FFUF: `go install github.com/ffuf/ffuf/v2@latest`

Massscan: `sudo apt-get --assume-yes install git make gcc ; git clone <https://github.com/robertdavidgraham/masscan> ; cd masscan ; make ; make install`

Dnmasscan: `git clone <https://github.com/rastating/dnmasscan.git`>

Brutespray: `apt-get install brutespray`****

HTTP screenshot: `apt-get install swig swig2.0 libssl-dev python-dev python-pip && git clone && cd httpscreenshot && pip install -r requirements.txt`

Nuclei: `go install -v github.com/projectdiscovery/nuclei/v2/cmd/nuclei@latest`

**Company recon**

Company acquisition data: [https://www.crunchbase.com/](https://www.crunchbase.com/)

ASN data: [https://bgp.he.net/](https://bgp.he.net/) || [https://github.com/j3ssie/metabigor](https://github.com/j3ssie/metabigor)

Amass: [https://github.com/owasp-amass/amass](https://github.com/owasp-amass/amass)

Reverse WHOIS: Get API Key → [https://www.whoxy.com/](https://www.whoxy.com/). `python3 domLink.py -D target.com -o target.out.txt`

Google/Baidu Dorking.

**Subdomain enumeration**

Hakrawler: [https://github.com/hakluke/hakrawler](https://github.com/hakluke/hakrawler)

SubDomainizer: [https://github.com/nsonaniya2010/SubDomainizer](https://github.com/nsonaniya2010/SubDomainizer)

SubFinder: [https://github.com/projectdiscovery/subfinder****](https://github.com/projectdiscovery/subfinder****)

Github Subdomains: [https://github.com/gwen001/github-subdomains.git](https://github.com/gwen001/github-subdomains.git) (Github API rate-limited. Add a 10s second sleep between each run)

[https://github.com/eth0izzle/shhgit/](https://github.com/eth0izzle/shhgit/)

**Subdomain brute-forcing**

FFUF: [https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)

**Alteration scanning**

Amass: [https://github.com/owasp-amass/amass](https://github.com/owasp-amass/amass)

**Port analysis**

Massscan: [https://github.com/robertdavidgraham/masscan](https://github.com/robertdavidgraham/masscan)

Dnmasscan: [https://github.com/rastating/dnmasscan](https://github.com/rastating/dnmasscan) ()

Nmap scan: [https://nmap.org/](https://nmap.org/)

Brutespray: [https://github.com/x90skysn3k/brutespray](https://github.com/x90skysn3k/brutespray)

**Github dorking**

Github-search: [https://github.com/gwen001/github-search.git](https://github.com/gwen001/github-search.git)

**Domain flyovers**

HTTP screenshot: [https://github.com/breenmachine/httpscreenshot](https://github.com/breenmachine/httpscreenshot)

**Domain takeovers**

can-I-take-over-xyz: [https://github.com/EdOverflow/can-i-take-over-xyz.git](https://github.com/EdOverflow/can-i-take-over-xyz.git)