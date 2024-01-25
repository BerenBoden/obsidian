##### ***Cracking SMTP***
You could modify the "From" address within the SMTP packet to make it appear as if the email is coming from a legitimate source. This is called Email Spoofing. Here are ways to do this:  
  
-Compromise an SMTP server directly, allowing you to send emails from it with spoofed "From" addresses  
-Write a script to automate the sending of emails with spoofed "From" addresses, using SMTP libraries in languages like Python, Perl or PHP  
-Configure an email client with a fake "From" address using vulnerable or Open Relay SMTP servers  
-If spamming a company and they have a web form on their website, you could manipulate the "From" address on the web form if it is not secure  
-Intercept and modify SMTP packets between the client and the server to alter the "From" address  
  
There are some protections in place against this however:  
  
-SPF (Sender Policy Framework)  
-DKIM (Domain-keys Policy Framework)  
-DMARC (Domain based Message Authentication, Reporting and Conformance)  
  
Not all servers are configured to use these, however, such as Open Relays or vulnerable servers.  
  
Besides changing the "From" address to avoid ending up in someone's spam folder, here's a few things you could do:  
  
-Use stolen email accounts for spamming  
-Use a compromised SMTP server to validate an email address for a domain, allowing it to appear trusted  
-Use a botnet to distribute the workload between IP addresses or do it manually. This spreads the reputation impact across multiple addresses instead of focusing it on a few or one (this is called Snowshoe Spamming)  
-Use Email Verification services to remove invalid or non-responsive email addresses to improve delivery rate, subsequently improving reputation  
-Send from an SMTP server that implements SPF, DKIM and DMARC to appear more legitimate  
-Avoid using words commonly used in spam messages. Don't do what others are doing  
-If targeting a specific individual, include information about the person within the email  
-Send simple emails that are likely to entice a response before sending the actual spam
##### ***Resources***
