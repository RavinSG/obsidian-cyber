---
tags:
  - Tool
---
Metasploit is the most widely used exploitation framework. Metasploit is a powerful tool that can *support all phases of a penetration testing* engagement, from information gathering to post-exploitation.

The Metasploit Framework is a set of tools that allow **information gathering**, **scanning**, **exploitation**, **exploit development**, **post-exploitation**, and more. While the primary usage of the Metasploit Framework focuses on the penetration testing domain, it is also useful for vulnerability research and exploit development.

The main components of the Metasploit Framework can be summarised as follows;

- **[[msfconsole]]**: The main command-line interface.
- **[[Modules]]**: supporting modules such as *exploits*, *scanners*, *payloads*, etc.
- **Tools**: Stand-alone tools that will help vulnerability research, vulnerability assessment, or penetration testing. Some of these tools are *msfvenom*, *pattern_create* and *pattern_offset*.

We have to work with the following recurring concepts when dealing with Metasploit.

- **[[Exploitation|Exploit]]**: A piece of code that uses a vulnerability present on the target system.
  
- **Vulnerability**: A design, coding, or logic flaw affecting the target system. The exploitation of a vulnerability can result in disclosing confidential information or allowing the attacker to execute code on the target system.
  
- **Payload**: An exploit will take advantage of a vulnerability. However, if we want the exploit to have the result we want (gaining access to the target system, read confidential information, etc.), we need to use a payload. Payloads are the code that will run on the target system.

[[Msfvenom]]

[[Meterpreter]]