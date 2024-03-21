---
URL: https://tryhackme.com/room/sqlinjectionlm
---
### In-Band SQL Injection
In-Band SQL Injection is the easiest type to detect and exploit; In-Band just refers to the same method of communication being used to exploit the vulnerability and also receive the results, for example, discovering an SQL Injection vulnerability on a website page and then *being able to extract data from the database to the same page*.

### Error-Based SQL Injection
This type of SQL Injection is the most useful for easily obtaining information about the database structure, as *error messages from the database are printed directly to the browser screen*. This can often be used to enumerate a whole database. 

- [[THM/Union-Based Injection|Union-Based Injection]]
- [[Blind SQL Injection]]

### Out-of-Band SQL Injection

Out-of-band SQL Injection isn't as common as it either depends on specific features being enabled on the database server or the web application's business logic, which makes some kind of external network call based on the results from an SQL query.

An Out-Of-Band attack is classified by having *two different communication channels*, one to launch the attack and the other to gather the results. For example, the attack channel could be a web request, and the data gathering channel could be monitoring HTTP/DNS requests made to a service you control.

1) An attacker makes a request to a website vulnerable to SQL Injection with an injection payload.

2) The Website makes an SQL query to the database, which also passes the hacker's payload.

3) The payload contains a request which forces an HTTP request back to the hacker's machine containing data from the database.

![[Out of Band SQLi.png]]