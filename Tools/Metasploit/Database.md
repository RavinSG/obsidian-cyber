
We will first need to start the PostgreSQL database, which Metasploit will use with the following command:  `systemctl start postgresql`

Then you will need to initialise the Metasploit Database using the `msfdb init` command. 

```bash
root@attackbox:~# systemctl start postgresql 
root@attackbox:~# msfdb init
[i] Database already started
[+] Creating database user 'msf'
[+] Creating databases 'msf'
[+] Creating databases 'msf_test'
[+] Creating configuration file '/usr/share/metasploit-framework/config/database.yml'
[+] Creating initial database schema
/usr/share/metasploit-framework/vendor/bundle/ruby/2.7.0/gems/activerecord-4.2.11.3/lib/active_record/connection_adapters/abstract_adapter.rb:84: warning: deprecated Object#=~ is called on Integer; it always returns nil
root@attackbox:~#
```

We can now launch `msfconsole` and check the database status using the `db_status` command.

### Workspaces

The database feature will allow us to create workspaces to *isolate different projects*. When first launched, we should be in the *default* workspace. We can list available workspaces using the `workspace` command.

```bash
msf6 > db_status
[*] Connected to msf. Connection type: postgresql.
msf6 > workspace 
* default
```

We can *add* a workspace using the `-a` parameter or *delete* a workspace using the `-d `parameter, respectively. 

```bash
sf6 > workspace -a tryhackme
[*] Added workspace: tryhackme
[*] Workspace: tryhackme
msf6 > workspace 
  default
* tryhackme
```

### Database Backend Commands

Different from regular Metasploit usage, once Metasploit is launched with a database, the `help` command, we will be shown the *Database Backends Commands* menu as well.

If you run a Nmap scan using the `db_nmap` shown below, all results will be saved to the database.

```bash
msf6 > db_nmap -sV -p- 10.10.12.229
[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2021-08-20 03:15 UTC
[*] Nmap: Nmap scan report for ip-10-10-12-229.eu-west-1.compute.internal (10.10.12.229)
[*] Nmap: Host is up (0.00090s latency).
[*] Nmap: Not shown: 65526 closed ports
[*] Nmap: PORT      STATE SERVICE            VERSION
[*] Nmap: 135/tcp   open  msrpc              Microsoft Windows RPC
[*] Nmap: 139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
[*] Nmap: 445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
[*] Nmap: 3389/tcp  open  ssl/ms-wbt-server?
[*] Nmap: 49152/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49153/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49154/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49158/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49162/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: MAC Address: 02:CE:59:27:C8:E3 (Unknown)
[*] Nmap: Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 94.91 seconds
msf6 >
```

### Hosts & Services

We can now reach information relevant to hosts and services running on target systems with the `hosts` and `services` commands, respectively. 

```bash
msf6 > hosts

Hosts
=====

address       mac                name                                        os_name  os_flavor  os_sp  purpose  info  comments
-------       ---                ----                                        -------  ---------  -----  -------  ----  --------
10.10.12.229  02:ce:59:27:c8:e3  ip-10-10-12-229.eu-west-1.compute.internal  Unknown                    device         

msf6 > services
Services
========

host          port   proto  name               state  info
----          ----   -----  ----               -----  ----
10.10.12.229  135    tcp    msrpc              open   Microsoft Windows RPC
10.10.12.229  139    tcp    netbios-ssn        open   Microsoft Windows netbios-ssn
10.10.12.229  445    tcp    microsoft-ds       open   Microsoft Windows 7 - 10 microsoft-ds workgroup: WORKGROUP
10.10.12.229  3389   tcp    ssl/ms-wbt-server  open   
10.10.12.229  49152  tcp    msrpc              open   Microsoft Windows RPC
10.10.12.229  49153  tcp    msrpc              open   Microsoft Windows RPC
10.10.12.229  49154  tcp    msrpc              open   Microsoft Windows RPC
10.10.12.229  49158  tcp    msrpc              open   Microsoft Windows RPC
10.10.12.229  49162  tcp    msrpc              open   Microsoft Windows RPC

msf6 >
```

The` hosts -h` and `services -h` commands can help us become more familiar with available options. 

Once the host information is stored in the database, we can use the `hosts -R` command to add this value to the RHOSTS parameter. 

```bash
msf6 > hosts -R

Hosts
=====

address       mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------       ---  ----  -------  ---------  -----  -------  ----  --------
10.10.44.147             Unknown                    device

RHOSTS => 10.10.44.147
```

The services command used with the `-S` parameter will allow you to search for specific services in the environment.

```bash
msf6 > services -S netbios                                                                                       
Services                                                                                                             
========  

host          port  proto  name         state  info                                                                              
----          ----  -----  ----         -----  ----                                                                              
10.10.12.229  139   tcp    netbios-ssn  open   Microsoft Windows netbios-ssn

msf6 >
```

