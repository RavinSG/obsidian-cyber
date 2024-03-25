---
URL: https://dnsdumpster.com
---

DNS lookup tools, such as *nslookup and dig, cannot find subdomains on their own*. The domain you are inspecting might include a different subdomain that can reveal much information about the target. 

For instance, if `tryhackme.com` has the subdomains `wiki.tryhackme.com` and `webmail.tryhackme.com`, you want to learn more about these two as they can hold a trove of information about your target. There is a *possibility that one of these subdomains* has been set up and is *not updated regularly*. Lack of proper regular updates usually leads to vulnerable services.

Instead of combing through multiple results from search engines, we can user a service like [DNSDumpster](https://dnsdumpster.com) to retrieve this information. If we search `DNSDumpster` for `tryhackme.com`, we will discover the subdomain `blog.tryhackme.com`, which a typical DNS query cannot provide. In addition, DNSDumpster will return the *collected DNS information in easy-to-read tables and a graph*. DNSDumpster will also provide any collected information about listening servers.

![[DNSDumpster.png]]

