---
aliases:
  - SSRF
tags:
  - Vulnerability
---
Server Side Request Forgery (**SSRF**) is a web vulnerability where an attacker manipulates a vulnerable application to make requests to internal or external resources on behalf of the server. This can lead to data exposure, unauthorised access to internal systems, or service disruptions.

There are two types of SSRF vulnerability; 

- The first is a regular SSRF where data is returned to the attacker's screen. 
- The second is a Blind SSRF vulnerability where an SSRF occurs, but no information is returned to the attacker's screen.

Assume the expected server request is as follow:
```http
http://website.thm/stock?server=api&id=123
```

Which translates to
```http
http://api.website.thm/api/stock/item?id=123
```

However, an attacker can use the `&x=` at the end of the payload to redirect the request:
```http
http://website.thm/stock?server=hacker-domain.thm/code?id=12&x=
```
which will now translate to

```http
http://hacker-domain.thm/code?id=12&x=.website.thm/api/stock/item?id=123
```

Since variables are delimited by `&`, the server will ignore the value assigned to `x` which is `.website.thm/api/stock/item?id=123`

Potential SSRF vulnerabilities can be spotted in web applications in many different ways.

![[Detect SSRF.png]]

### Defences against SSRF
