
**tcpdump** is a command-line network protocol analyser. It is popular, lightweight–meaning it *uses little memory and has a low CPU usage*–and uses the open-source libpcap library. tcpdump is text based, meaning all commands in tcpdump are executed in the terminal. It can also be installed on other Unix-based operating systems, such as macOS®. It is preinstalled on many Linux distributions.

tcpdump provides a brief packet analysis and converts key information about network traffic into formats easily read by humans. It prints information about each packet directly into your terminal. tcpdump also displays the source IP address, destination IP addresses, and the port numbers being used in the communications. 

## Interpreting output

tcpdump prints the output of the command as the sniffed packets in the command line, and optionally to a log file, after a command is executed. The output of a packet capture contains many pieces of important information about the network traffic. 

![[tcpdump.png]]

Some information you receive from a packet capture includes: 

- **Timestamp**: The output begins with the timestamp, formatted as hours, minutes, seconds, and fractions of a second.  

- **Source IP**: The packet’s origin is provided by its source IP address.

- **Source port**: This port number is where the packet originated.

- **Destination IP**: The destination IP address is where the packet is being transmitted to.

- **Destination port**: This port number is where the packet is being transmitted to.

## Common uses

tcpdump and other network protocol analysers are commonly used to *capture and view network communications* and to *collect statistics about the network*, such as troubleshooting network performance issues. They can also be used to:

- Establish a baseline for network traffic patterns and network utilisation metrics.

- Detect and identify malicious traffic

- Create customised alerts to send the right notifications when network issues or security threats arise.

- Locate unauthorised instant messaging (IM), traffic, or wireless access points.

However, **attackers** can also use network protocol analysers **maliciously to gain information** about a specific network. For example, attackers can capture data packets that contain sensitive information, such as account usernames and passwords. 

## Example Capture

Consider a user checking his email messages using **POP3**. First, we are going to use Tcpdump to attempt to capture the username and password. In the terminal output below, we used the command `sudo tcpdump port 110 -A`.

We need `sudo` as *packet captures require root privileges*. We wanted to limit the number of captured and displayed packets to those exchanged with the POP3 server. We know that *POP3 uses port 110*, so we filtered our packets using `port 110`. Finally, we wanted to *display the contents* of the captured packets in ASCII format, so we added `-A`.

```bash
sudo tcpdump -i tun0 port 110 -A
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on tun0, link-type RAW (Raw IP), snapshot length 262144 bytes
10:00:43.074011 IP kali.53386 > 10.10.148.119.pop3: Flags [S], seq 2593839447, win 32120, options [mss 1460,sackOK,TS val 3603448958 ecr 0,nop,wscale 7], length 0
E..<U7@.@...
.Hs

.w
.Hs.n.........X...........
~4jE..E.+OK Hello there.

10:00:43.653633 IP kali.53386 > 10.10.148.119.pop3: Flags [.], ack 19, win 251, options [nop,nop,TS val 3603449537 ecr 2117364293], length 0
E..4U9@.@...
.Hs

.w...n...X...............
..F.~4jE
10:00:49.044701 IP kali.53386 > 10.10.148.119.pop3: Flags [P.], seq 1:13, ack 19, win 251, options [nop,nop,TS val 3603454928 ecr 2117364293], length 12
E..@U:@.@...
.Hs

.w...n...X...............
..[.~4jEuser frank

.w
.Hs.n.........d...._......
~4.r..[.+OK Password required.

10:00:49.328902 IP kali.53386 > 10.10.148.119.pop3: Flags [.], ack 43, win 251, options [nop,nop,TS val 3603455212 ecr 2117369970], length 0
E..4U;@.@...
.Hs

.w...n...d........b......
..\.~4.r
10:00:52.866112 IP kali.53386 > 10.10.148.119.pop3: Flags [P.], seq 13:28, ack 43, win 251, options [nop,nop,TS val 3603458750 ecr 2117369970], length 15
E..CU<@.@...
.Hs

.w...n...d........)k.....
..j.~4.rpass D2xc9CgD
```

In the terminal output above, we can see the username and password were each sent in their own packet. The first packet explicitly displays “**USER frank**”, while the last packet reveals the password “**PASS D2xc9CgD**”.

