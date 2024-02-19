
**tcpdump** is a command-line network protocol analyser. It is popular, lightweight–meaning it *uses little memory and has a low CPU usage*–and uses the open-source libpcap library. tcpdump is text based, meaning all commands in tcpdump are executed in the terminal. It can also be installed on other Unix-based operating systems, such as macOS®. It is preinstalled on many Linux distributions.

tcpdump provides a brief packet analysis and converts key information about network traffic into formats easily read by humans. It prints information about each packet directly into your terminal. tcpdump also displays the source IP address, destination IP addresses, and the port numbers being used in the communications. 

### Interpreting output

tcpdump prints the output of the command as the sniffed packets in the command line, and optionally to a log file, after a command is executed. The output of a packet capture contains many pieces of important information about the network traffic. 

![[tcpdump.png]]

Some information you receive from a packet capture includes: 

- **Timestamp**: The output begins with the timestamp, formatted as hours, minutes, seconds, and fractions of a second.  

- **Source IP**: The packet’s origin is provided by its source IP address.

- **Source port**: This port number is where the packet originated.

- **Destination IP**: The destination IP address is where the packet is being transmitted to.

- **Destination port**: This port number is where the packet is being transmitted to.

### Common uses

tcpdump and other network protocol analysers are commonly used to *capture and view network communications* and to *collect statistics about the network*, such as troubleshooting network performance issues. They can also be used to:

- Establish a baseline for network traffic patterns and network utilisation metrics.

- Detect and identify malicious traffic

- Create customised alerts to send the right notifications when network issues or security threats arise.

- Locate unauthorised instant messaging (IM), traffic, or wireless access points.

However, **attackers** can also use network protocol analysers **maliciously to gain information** about a specific network. For example, attackers can capture data packets that contain sensitive information, such as account usernames and passwords. 