- [[Passive Reconnaissance]]
### Hunting Subdomains

One great tool can be used is `sublist3r`. Another is the website [crt.sh](https://crt.sh) that uses certificate fingerprinting to find subdomains.

A more modern tool is [project-amass](https://github.com/owasp-amass/amass) by OWASP.

### Identifying Website Technologies

- [builtwith](https://builtwith.com) -  A website to list down what technologies are used to build a site
- [Wappalyzer](https://www.wappalyzer.com)- A plugin that can be used to check a website for the tech stack used
- whatweb - A builtin tool in Kali to query websites 
- Burp Suite - Burp Suite can be used to intercept requests and responses to look at the headers to gather information

### Google Fu

You can also use google with search modifiers such as site:, filetype: to narrow down the search. This will also list down subdomains.