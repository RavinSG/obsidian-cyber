---
aliases:
  - NSE
---
Nmap provides support for scripts using the *Lua language*. A part of Nmap, Nmap Scripting Engine (**NSE**) is a Lua interpreter that allows Nmap to execute Nmap scripts written in Lua language. 

Nmap default installation can easily contain *close to 600 scripts*. These scripts are located at` /usr/share/nmap/scripts`, and you will notice that there are hundreds of scripts conveniently named starting with the protocol they target.

We can specify to use any or a group of these installed scripts; moreover, you can install other user’s scripts and use them for your scans. Let’s begin with the **default scripts**. You can choose to run the scripts in the default category using `--script=default` or simply adding `-sC`.

In addition to default, categories include *auth*, *broadcast*, *brute*, *default*, *discovery*, *dos*, *exploit*, *external*, *fuzzer*, *intrusive*, *malware*, *safe*, *version*, and *vuln*. A brief description is shown in the following table.

| Script Category  | Description                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| `auth`           | Authentication related scripts                                         |
| `broadcast`	<br> | Discover hosts by sending broadcast messages                           |
| `brute`	<br>     | Performs brute-force password auditing against logins                  |
| `default`	<br>   | Default scripts, same as `-sC`                                         |
| `discovery`	<br> | Retrieve accessible information, such as database tables and DNS names |
| `dos`	<br>       | Detects servers vulnerable to Denial of Service (DoS)                  |
| `exploit`	<br>   | Attempts to exploit various vulnerable services                        |
| `external`	<br>  | Checks using a third-party service, such as Geoplugin and Virustotal   |
| `fuzzer`	<br>    | Launch fuzzing attacks                                                 |
| `intrusive`	<br> | Intrusive scripts such as brute-force attacks and exploitation         |
| `malware`	<br>   | Scans for backdoors                                                    |
| `safe`	<br>      | Safe scripts that won’t crash the target                               |
| `version`	<br>   | Retrieve service versions                                              |
| `vuln`           | Checks for vulnerabilities or exploit vulnerable services              |

Some scripts belong to more than one category. Moreover, some scripts launch brute-force attacks against services, while others launch DoS attacks and exploit systems. Hence, it is crucial to be *careful when selecting scripts* to run if you don’t want to crash services or exploit them.

We can specify the script by name using `--script "SCRIPT-NAME"` or a pattern such as `--script "ftp*"`, which would include `ftp-brute`. If you are unsure what a script does, you can open the script file with a text reader, such as `less`, or a text editor. 

In the case of `ftp-brute`, it states: “Performs brute force password auditing against FTP servers.” You have to be careful as some scripts are pretty intrusive. Moreover, some scripts might be for a specific server and, if chosen at random, will waste your time with no benefit. As usual, make sure that you are authorised to launch such tests on the target server.



