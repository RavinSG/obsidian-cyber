
This tool runs automatically with the [[IPv6 DNS Takeover]] attack. However, if we compromise a domain in another way and need to dump the information, we need to run this manually.

```bash
$ sudo /usr/bin/ldapdomaindump ldaps://192.168.23.130 -u 'MARVEL\fcastle' -p Password1 
[*] Connecting to host...
[*] Binding to host
[+] Bind OK
[*] Starting domain dump
[+] Domain dump finished

$ ls               
domain_computers_by_os.html  domain_computers.json  domain_groups.json  domain_policy.json  domain_trusts.json          domain_users.html
domain_computers.grep        domain_groups.grep     domain_policy.grep  domain_trusts.grep  domain_users_by_group.html  domain_users.json
domain_computers.html        domain_groups.html     domain_policy.html  domain_trusts.html  domain_users.grep
```

> [!note] 
> Need to include the full path for ladpdomaindump since it was throwing a 'ValueError: unsupported hash type MD4' error.

