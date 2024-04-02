UDP is a connectionless protocol, and hence it does not require any handshake for connection establishment. We *cannot guarantee that a service listening* on a UDP port would respond to our packets. However, **if a UDP packet is sent to a closed port, an ICMP port unreachable error** (type 3, code 3) is returned. You can select UDP scan using the `-sU` option; moreover, you can combine it with another TCP scan.

![[UDP Scan.png]]

We can launch a UDP scan as follow:

```bash
sudo nmap -sU -F -v 10.10.161.118
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-02 11:01 AEDT
Initiating Ping Scan at 11:01
Scanning 10.10.161.118 [4 ports]
Completed Ping Scan at 11:01, 0.31s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 11:01
Completed Parallel DNS resolution of 1 host. at 11:01, 0.05s elapsed
Initiating UDP Scan at 11:01
Scanning 10.10.161.118 [100 ports]
Discovered open port 53/udp on 10.10.161.118
Increasing send delay for 10.10.161.118 from 0 to 50 due to max_successful_tryno increase to 4
Increasing send delay for 10.10.161.118 from 50 to 100 due to max_successful_tryno increase to 5
Increasing send delay for 10.10.161.118 from 100 to 200 due to max_successful_tryno increase to 6
Increasing send delay for 10.10.161.118 from 200 to 400 due to 11 out of 11 dropped probes since last increase.
UDP Scan Timing: About 49.00% done; ETC: 11:02 (0:00:32 remaining)
Discovered open port 111/udp on 10.10.161.118
Increasing send delay for 10.10.161.118 from 400 to 800 due to 11 out of 19 dropped probes since last increase.
Completed UDP Scan at 11:03, 103.18s elapsed (100 total ports)
Nmap scan report for 10.10.161.118
Host is up (0.32s latency).
Not shown: 97 closed udp ports (port-unreach)
PORT    STATE         SERVICE
53/udp  open          domain
68/udp  open|filtered dhcpc
111/udp open          rpcbind

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 103.66 seconds
           Raw packets sent: 303 (17.246KB) | Rcvd: 110 (9.064KB)

```

The `-F` flag enables fast mode to only scan for few known ports.