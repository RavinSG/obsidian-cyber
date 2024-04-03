
In the same sense that an IP address specifies a host on a network among many others, a TCP port or UDP port is used to identify a network service running on that host. A server provides the network service, and it *adheres to a specific network protocol*. Examples include **providing time**, **responding to DNS queries**, and **serving web pages**.

We can generally classify ports in two states:

- **Open port** indicates that there is some service listening on that port.
- **Closed port** indicates that there is no service listening on that port.

However, in practical situations, we need to consider the *impact of firewalls*. For instance, a port might be open, but a firewall might be blocking the packets. Therefore, Nmap considers the following *six states:

- **Open**: indicates that a *service is listening* on the specified port
- **Closed**: indicates that *no service is listening* on the specified port, although the port is accessible. By accessible, we mean that it is reachable and is not blocked by a firewall or other security appliances/programs
- **Filtered**: means that Nmap cannot determine if the port is open or closed because the *port is not accessible*. This state is usually *due to a firewall* preventing Nmap from reaching that port. Nmap’s packets may be blocked from reaching the port; alternatively, the responses are blocked from reaching Nmap’s host
- **Unfiltered**: means that Nmap *cannot determine* if the port is open or closed, although the *port is accessible*. This state is encountered when using an ACK scan `-sA`
- **Open|Filtered**: This means that Nmap cannot determine whether the port is *open or filtered*.
- **Closed|Filtered**: This means that Nmap cannot decide whether a port is *closed or filtered*.

#### Scan types 
We can use the [[TCP Flags]] to perform various kinds of scans.

- [[TCP Connect Scan]]
- [[TCP SYN Scan]]
- [[TCP ACK Scan]]
- [[Window Scan]]
- [[Null Scan]]
- [[FIN Scan]]
- [[Xmas Scan]]
- [[Maimon Scan]]
- [[Custom Scan]]
- [[UDP Scan]]

One scenario where **Null**, **FIN**, and **Xmas** scan types can be efficient is when scanning a target *behind a [[Firewall#^stateless|stateless]] (non-stateful) firewall*. A stateless firewall will check if the incoming packet has the *SYN flag set* to detect a connection attempt. Using a flag combination that does not match the SYN packet makes it possible to deceive the firewall and reach the system behind it. However, **a stateful firewall will practically block all such crafted packets** and render this kind of scan useless.
### Fine-Tuning Scope and Performance

We can specify the ports we want to scan instead of the default 1000 ports. Specifying the ports is intuitive by now. Let’s see some examples:

- port list: `-p22,80,443` will scan ports `22`, `80` and `443`.
- port range: -`p1-1023` will scan all ports *between 1 and 1023 inclusive*.
- We can request the scan of *all ports* by using `-p-`, which will *scan all 65535 ports*. 
- If we want to scan the most *common 100 ports*, add `-F`. Using `--top-ports 10` will check the **ten most common ports**.

#### [[Spoofing and Decoy]]

#### [[Idle-Zombie Scan|Idle/Zombie Scan]]

### Getting More Details

We might consider adding `--reason` if we want Nmap to provide more details regarding its reasoning and conclusions. Consider the two scans below to the system; however, the latter adds `--reason`.

```bash
pentester@TryHackMe$ sudo nmap -sS 10.10.252.27

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:39 BST
Nmap scan report for ip-10-10-252-27.eu-west-1.compute.internal (10.10.252.27)
Host is up (0.0020s latency).
Not shown: 994 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
110/tcp open  pop3
111/tcp open  rpcbind
143/tcp open  imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.60 seconds
```

```bash
pentester@TryHackMe$ sudo nmap -sS --reason 10.10.252.27

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:40 BST
Nmap scan report for ip-10-10-252-27.eu-west-1.compute.internal (10.10.252.27)
Host is up, received arp-response (0.0020s latency).
Not shown: 994 closed ports
Reason: 994 resets
PORT    STATE SERVICE REASON
22/tcp  open  ssh     syn-ack ttl 64
25/tcp  open  smtp    syn-ack ttl 64
80/tcp  open  http    syn-ack ttl 64
110/tcp open  pop3    syn-ack ttl 64
111/tcp open  rpcbind syn-ack ttl 64
143/tcp open  imap    syn-ack ttl 64
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.59 seconds
```

Providing the` --reason` flag gives us the *explicit reason why Nmap concluded* that the system is up or a particular port is open. In this console output above, we can see that this system is considered online because Nmap “received arp-response.” 

On the other hand, we know that the SSH port is deemed to be open because Nmap received a “syn-ack” packet back.

#### [[Service Detection]]

