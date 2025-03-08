
For this attack to work, SMB signing should be **disabled or not enforced** on the target. In addition, the relayed credentials **must be a local admin** of the machine that is attacked. The credentials **cannot be relayed to itself**.

### Identify hosts without SMB signing enabled

#### Domain Controller

```bash
$ sudo nmap --script=smb2-security-mode.nse -p445 192.168.23.130
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-18 11:03 AEDT
Nmap scan report for 192.168.23.130
Host is up (0.00090s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:D5:BC:80 (VMware)

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Nmap done: 1 IP address (1 host up) scanned in 0.20 seconds
```

Domain controllers require SMB signing to be turned on, hence we cannot use the relay attack on the DC. However, this is not turned on by default for users.

#### Machine 1 & 2

```bash
$ sudo nmap --script=smb2-security-mode.nse -p445 192.168.23.131
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-18 11:03 AEDT
Nmap scan report for 192.168.23.131
Host is up (0.00085s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:AB:9C:5D (VMware)

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Nmap done: 1 IP address (1 host up) scanned in 0.27 seconds

$ sudo nmap --script=smb2-security-mode.nse -p445 192.168.23.132
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-18 11:03 AEDT
Nmap scan report for 192.168.23.132
Host is up (0.00042s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:79:43:05 (VMware)

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Nmap done: 1 IP address (1 host up) scanned in 0.22 seconds
```

We can see that even though SMB signing is enabled, it is not required for these two machines. Hence we can run the attack against them.  

You can simply scan the full subnet with the command:
	`sudo nmap --script=smb2-security-mode.nse -p445 192.168.23.0/24`

Once we have identified the hosts without signing, we can create a `target.txt` file with the ip addresses of the hosts.

### Configure and run Responder

We need to turn off **SMB** and **HTTP** to make sure the hashes are not only captured, but relayed as well. The config file is located at `/etc/responder/Responder.conf`. Once they are turned off, we can verify it by running responder.

![[Responder SMB Relay.png]]

### Setup ntlmrelayx

Once the Responder catches a hash, it will be forwarded to the ntlmrelayx, which will then forward it to the selected target. Since we need a network event, we point machine 1 towards the attacker.  We get the following output from ntlmrelayx. ^setupNtlmrelayx

![[ntlmrelayx output.png]] ^c05678

We can see the first attempt was to relay the hash to `192.168.23.131`. However, since this is the same machine the hash originated from, the attack fails. 

The same user is a local admin in `192.168.23.132`, hence the relay attack against that machine succeeds and the SAM hashes are dumped. 

We can a `-i` to get an interactive shell using ntlmrelayx instead of getting the SAM (Security Account Manager) dump.

![[nltmrelayx interactive.png]]


### Additional Info

[[SMB]]

using `ntlmrelayx.py` -  https://www.secureauth.com/resources/we-love-relaying-credentials-a-technical-guide-to-relaying-credentials-everywhere/

### Defences

- Enable SMB Signing on all devices
	- *Pro*: Completely stops the attack
	- **Con**: Can cause performance issues with file copies
	  
- Disable NLTM authentication on network
	- *Pro*: Completely stops the attack
	- **Con**: If Kerberos stops working, Windows will default back to NLTM
	  
- Account Tiering
	- *Pro*: Limits domain admins to specific tasks (e.g. only log onto servers with need for DA)
	- **Con**: Enforcing the policy may be difficult
- Local Admin Restriction:
  
	- *Pro*: Can prevent a lot of lateral movement
	- **Con**: Potential increase in the amount of service desk tickets