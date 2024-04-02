
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

### [[Spoofing and Decoy]]