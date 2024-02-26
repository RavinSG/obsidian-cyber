---
tags:
  - Attack
---

Subdomain enumeration is the process of finding valid subdomains for a domain. This is done to expand the attack surface to try and discover more potential points of vulnerability.

There are three different subdomain enumeration methods:

- Brute Force
- [[Open-Source Intelligence|OSINT]]
- Virtual Host

### SSL/TLS Certificates (OSINT)

When an [[Secure Sockets Layer|SSL/TLS]] (Secure Sockets Layer/Transport Layer Security) certificate is created for a domain by a **CA** (Certificate Authority), CA's take part in what's called "*Certificate Transparency (CT) logs*". These are publicly accessible logs of every SSL/TLS certificate created for a domain name. 

The purpose of Certificate Transparency logs is to stop malicious and accidentally made certificates from being used. We can use this service to our advantage to discover subdomains belonging to a domain, sites like https://crt.sh and https://ui.ctsearch.entrust.com/ui/ctsearchui offer a searchable database of certificates that shows current and historical results.

### Search Engines (OSINT)

Search engines contain trillions of links to more than a billion websites, which can be an excellent resource for finding new subdomains. Using advanced search methods on websites like Google, such as the site: filter, can narrow the search results. For example, "-site:www.domain.com site:\*.domain.com" would only contain results leading to the domain name domain.com but exclude any links to www.domain.com; therefore, it shows us only subdomain names belonging to domain.com.

To speed up the process of *OSINT subdomain discovery*, we can automate the above methods with the help of tools like **Sublist3r**.
### DNS Bruteforce

Bruteforce **[[Domain Name System|DNS]]** (Domain Name System) enumeration is the method of *trying tens, hundreds, thousands or even millions of different possible subdomains* from a pre-defined list of commonly used subdomains. Because this method requires many requests, we automate it with tools to make the process quicker. In this instance, we can use a tool called **dnsrecon** to perform this. 

### Virtual Hosts

Some subdomains aren't *always hosted in publicly accessible DNS results*, such as development versions of a web application or administration portals. Instead, the **DNS record could be kept on a private DNS server** or recorded on the developer's machines in their /etc/hosts file (or c:\\windows\\system32\\drivers\\etc\\hosts file for Windows users) which maps domain names to IP addresses. 

Because web servers can host multiple websites from one server when a website is requested from a client, the server knows which website the client wants from the Host header. We can utilise this host header by making changes to it and monitoring the response to see if we've discovered a new website.

```bash
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u {Target URL}
```

The above command uses the `-w` switch to *specify the wordlist* we are going to use. The `-H `switch *adds/edits a header* (in this instance, the Host header), we have the FUZZ keyword in the space where a subdomain would normally go, and this is where we will try all the options from the wordlist.

Because the above command will always produce a valid result, we need to filter the output. We can do this by *using the page size* result with the `-fs` switch. This switch, tells ffuf to ignore any results that are of the specified size.

```bash
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u {Target URL} -fs {size}
```
