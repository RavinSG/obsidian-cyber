---
tags:
  - Attack
---
In some scenarios, web applications are written to *request access to files* on a given system, including images, static text, and so on *via parameters*. Parameters are query parameter strings attached to the URL that could be used to retrieve data or perform actions based on user input. The following diagram breaks down the essential parts of a URL.

Let's discuss a scenario where a user requests to access files from a web-server. First, the user sends an HTTP request to the web-server that includes a file to display. For example, if a user wants to access and display their CV within the web application, the request may look as follows, `http://webapp.thm/get.php?file=userCV.pdf`, where the `file` is the *parameter* and the `userCV.pdf`, is the *required file* to access.﻿

### Why do File inclusion vulnerabilities happen?﻿
File inclusion vulnerabilities are commonly found and exploited in various programming languages for web applications, such as PHP that are poorly written and implemented. The main issue of these *vulnerabilities is the input validation*, in which the user inputs are not sanitised or validated, and the user controls them. When the input is not validated, the user can pass any input to the function, causing the vulnerability.

### What is the risk of File inclusion?
By default, an attacker can leverage file inclusion vulnerabilities to *leak data, such as code, credentials* or other important files related to the web application or operating system. Moreover, if the attacker can write files to the server by any other means, file inclusion might be used in tandem to gain remote command execution (RCE).

### Path Traversal
Also known as **Directory traversal**, a web security vulnerability allows an attacker to *read operating system resources*, such as local files on the server running an application. The attacker exploits this vulnerability by manipulating and abusing the web application's URL to locate and access files or directories stored outside the application's root directory.

Path traversal vulnerabilities occur when the user's input is passed to a function such as `file_get_contents` in PHP. It's important to note that the function is not the main contributor to the vulnerability. Often poor input validation or filtering is the cause of the vulnerability.

The following graph shows how a web application stores files in `/var/www/app`. The happy path would be the user requesting the contents of userCV.pdf from a defined path `/var/www/app/CVs`.

![[Path Traversal.png]]

We can test out the URL parameter by adding payloads to see how the web application behaves. Path traversal attacks, also known as the **dot-dot-slash** attack, take advantage of moving the directory one step up using the double dots `../`. If the attacker finds the entry point, which in this case `get.php?file=`, then the attacker may send something as follows, `http://webapp.thm/get.php?file=../../../../etc/passwd`.

Suppose there isn't input validation, and instead of accessing the PDF files at `/var/www/app/CVs` location, the web application retrieves files from other directories, which in this case `/etc/passwd`. 

### Local File Inclusion (﻿[[Local File Inclusion|LFI]])

### Remote File Inclusion ([[Remote File Inclusion|RFI]])

### Preventing File Inclusion

To prevent the file inclusion vulnerabilities, some common suggestions include:

- Keep system and services, including web application frameworks, updated with the latest version.
- Turn off PHP errors to avoid leaking the path of the application and other potentially revealing information.
- A Web Application Firewall (**WAF**) is a good option to help mitigate web application attacks.
- Disable some PHP features that cause file inclusion vulnerabilities if your web app doesn't need them, such as `allow_url_fopen` on and `allow_url_include`.
- Carefully analyse the web application and allow only protocols and PHP wrappers that are in need.
- Never trust user input, and make sure to implement proper input validation against file inclusion.
- Implement whitelisting for file names and locations as well as blacklisting