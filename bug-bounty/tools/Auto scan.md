##### ***Workflow***
There are 4 main features of the auto-scan project, the first is a RabbitMQ server that runs on a master server, it uses a NodeJS API to store data in a PSQL database.

DB Models:
`responses`: Many-to-One relation with `domains` Data collected from the response.
- response_id INT PRIMARY KEY
- created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
- status-code INT
- content-length INT
- technology VARCHAR (255) NOT NULL
- redirect locations [(string)]
- body hash (md5,mmh3,simhash,sha1,sha256,sha512) (string)
- favicon hash (md5,mmh3,simhash,sha1,sha256,sha512) (string)
- time-to-first-byte (double)
- request method (string)
- response IP address (string)
- body line count (number)
- websocket (string)
- tls (boolean)
- asn (string)
- body word count (number)
- body content (string)
- headers [(string)]
- cookies [(string)]
- jarm fingerprint hash (string)
- response header size (number)
- response time (number)
- waf (string)
- http version (string)
- mime type (string)
- endpoint (string)
- screenshot_id INT
- FOREIGN KEY (screenshot_id) REFERENCES screenshots(screenshot_id)
- filter_id INT (if exists, then the values of this item will be set on the filters for each scanning tool)
- FOREIGN KEY (filter_id) REFERENCES filters(filter_id)
- domain_id INT
- FOREIGN KEY (domain_id) REFERENCES domains(domain_id)

`domains`: Main domains to run initial scans against.
- domain_id INT PRIMARY KEY
- tool [(string)]
- name (string)
- scope (string)
- extracted_links_id INT
- FOREIGN KEY (extracted_links_id) REFERENCES extracted_links(extracted_links_id)
- permutation_id INT
- FOREIGN KEY (permutation_id) REFERENCES permutations(permutation_id)
- dns_id INT
- FOREIGN KEY (dns_id) REFERENCES dns(dns_id)

`filters`: Any response data that should be ignored or only scanned are manually entered here, and filtered out or in before conducting any scans.
- filter_id INT PRIMARY KEY
- response_id INT
- filtered (boolean) (if filtered is true, then exclude those responses, else filter for only those responses)
- rate limit (req per second)
- active (boolean) (if active is true then the filter is used on the tool, else ignored)
- FOREIGN KEY (response_id) REFERENCES responses(response_id)

`screenshots`: Relation to `domains`. Screenshot of live web page.

`dns`: Relation to `domains`
- id INT PRIMARY KEY
- a records INET[]
- aaaa records INET[]
- cname records[]
- mx records[]
- txt records[]
- ns records[]
- srv records[]
- ptr records[]
- spf records[]
- soa records[]

`permutations`: Relation to  `domains`. Any permutations created.
- permutation_id INT PRIMARY KEY
- permutation (string)

`word_lists`: Word lists that will be used for scans. Any domains inside of `domains` are excluded.
- id INT PRIMARY KEY
- word VARCHAR (255) UNIQUE NOT NULL
- endpoint VARCHAR (255) UNIQUE NOT NULL

`nmap_results`: Relation to `dns`. 
- id INT PRIMARY KEY
- result TEXT
- dns_id INT
- FOREIGN KEY (dns_id) REFERENCES dns(dns_id)

`extracted_links[]`: Relation to `domains` where root_domain is true. Any links or domains extracted from JavaScript or HTML files, cloud links, dorks, etc.... This is for any domains that do not match the format of a subdomain or do not include the seed domain, but they come from a response from the application, or from a tool scanning the cloud or internet.
- id INT PRIMARY KEY
- link VARCHAR (255) UNIQUE NOT NULL

The flow goes as follows:
1. Passive mode. 
	- RabbitMQ master server takes a list of domains from `domains.txt`, adds each one to the `domains` table. A copy of all `responses` values are created in memory as a variable.
	- RabbitMQ master server evenly distributes each value in the `domains` table out to each worker.
	- RabbitMQ worker server receives list of `domains` values, for each value it runs amass, asset finder and subfinder to scan for sub domains passively. List of found domains for each tool is added to files under /temp. Each file is enumerated, a list of current domains from the `domains` table are compared with the new list of enumerated domains and piped into notify to notify slack. and added to `domains` table with associated tool. 
	- RabbitMQ worker runs auto dorking scripts, waybackurls, , etc... Any found domains are added to `domains` or `extracted_links` if the found URL does not contain a regex value of a root_domain true domain value.
	- RabbitMQ worker runs link finder tool to extract any links found from the body_content of `responses` values.
	- RabbitMQ worker creates subdomain and endpoint permutations from `domains` based on the technology stack and commonly used words & custom headers. Adds to `permutations` table.
	- RabbitMQ worker uses httpx to probe`domains` and gather response information, any found response information is added to the `response` model.
	- RabbitMQ worker uses gowitness to take screenshots of any domains with a [200,403] status code, or endpoints with activated filters.
	- RabbitMQ master waits until the worker has finished its current task, then compares the `responses` table values to the values that were saved in memory before the worker started the scan (new response data was added to db from the worker scan task). Pipes any results and new values into notify and sends to slack.
2. Active mode
	- 


{
    "names": ["zomato.com","api.grofers.com","hyperpure.com","api2.grofers.com","blinkit.com", "winecellar.zomato.com","zdev.net","grofer.io","zomans.com","runnr.in"],
    "scope": "zomato"
}