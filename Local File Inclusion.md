---
aliases:
  - LFI
tags:
  - Vulnerability
---
Local File Inclusion (**LFI**) is a security vulnerability that occurs when a *web application allows users to include files from the local file system*. Attackers exploit LFI by manipulating input fields to retrieve sensitive files or execute malicious code. This can lead to unauthorised access to system files, data leakage, and potential remote code execution.

**LFI** attacks against web applications are often due to a developers' lack of security awareness. With *PHP*, using functions such as `include`, `require`, `include_once`, and `require_once` often contribute to vulnerable web applications. However, it's worth noting LFI vulnerabilities also occur when using other languages such as *ASP*, *JSP*, or even in *Node.js* apps. LFI exploits follow the same concepts as path traversal.

#### Suppose the web application provides two languages, and the user can select between the EN and AR

```PHP
<?PHP 
	include($_GET["lang"]);
?>
```

The PHP code above uses a GET request via the URL parameter lang to include the file of the page. The call can be done by sending the following HTTP request as follows: `http://webapp.thm/index.php?lang=EN.php` to load the English page or `http://webapp.thm/index.php?lang=AR.php` to load the Arabic page, where EN.php and AR.php files exist in the same directory.

Theoretically, we can access and display any readable file on the server from the code above if there isn't any input validation. Let's say we want to read the `/etc/passwd` file, which contains sensitive information about the users of the Linux operating system, we can try the following:`http://webapp.thm/get.php?lang=/etc/passwd 
`
In this case, it works because there *isn't a directory specified in the include function and no input validation*.

####  The developer decided to specify the directory inside the function

```PHP
<?PHP 
	include("languages/". $_GET['lang']); 
?>
```

In the above code, the developer decided to use the *include* function to call PHP pages in the languages directory only via lang parameters.

If there is no input validation, the attacker can manipulate the URL by replacing the lang input with other OS-sensitive files such as `/etc/passwd`.

Again the payload looks similar to the path traversal, but the include function allows us to include any called files into the current page. The following will be the exploit: `http://webapp.thm/index.php?lang=../../../../etc/passwd`

#### Black box testing

In a Black box setting we don't have the source code. In this case, errors are significant in understanding how the data is passed and processed into the web app.

In this scenario, we have the following entry point: `http://webapp.thm/index.php?lang=EN`. If we enter an invalid input, such as THM, we get the following error

```PHP
Warning: include(languages/THM.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12
```

The error message discloses significant information. By entering *THM* as input, an error message shows what the include function looks like:  `include(languages/THM.php)`;. 

If you look at the directory closely, we can tell the function includes files in the languages directory is adding  `.php` at the end of the entry.

Using *null bytes is an injection technique* where URL-encoded representation such as `%00` or `0x00` in hex with user-supplied data to *terminate strings*. You could think of it as trying to trick the web app into disregarding whatever comes after the Null Byte.

By adding the Null Byte at the end of the payload, we tell the  include function to ignore anything after the null byte which may look like:

`include("languages/../../../../../etc/passwd%00").".php")`; which equivalent to → `include("languages/../../../../../etc/passwd")`;

#### Keyword Filtering

The developer decided to filter keywords to avoid disclosing sensitive information! The `/etc/passwd` file is being filtered. There are two possible methods to bypass the filter. First, by using the NullByte `%00` or the current directory trick at the end of the filtered keyword `/.`

#### Input Validation

When we enter `http://webapp.thm/index.php?lang=../../../../etc/passwd`. We got the following error!

```PHP
Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/THM-5/index.php on line 15
```
 
If we check the warning message in the include`(languages/etc/passwd)` section, we know that the web application replaces the `../` with the empty string. There are a couple of techniques we can use to bypass this.

We can send the following payload to bypass it: 
`....//....//....//....//....//etc/passwd`

This works because the PHP filter *only matches and replaces the first subset string* `../` it finds and doesn't do another pass, leaving what is pictured below.

![[PHP Input Validation.png]]

