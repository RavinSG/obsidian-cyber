The TCP header is the first 24 bytes of a TCP segment. The following figure shows the TCP header as defined in RFC 793.

![[TCP Header.png]]

The TCP flags are highlighted in red. Setting a flag bit means setting its value to 1. From left to right, the TCP header flags are:

- **URG**: Urgent flag indicates that the urgent pointer filed is significant. The urgent pointer indicates that the *incoming data is urgent*, and that a TCP segment with the URG flag set is processed immediately without consideration of having to wait on previously sent TCP segments.
  
- **ACK**: Acknowledgement flag indicates that the acknowledgement number is significant. It is used to *acknowledge the receipt* of a TCP segment.
  
- **PSH**: Push flag asking TCP to *pass the data* to the application promptly.
  
- **RST**: Reset flag is used to *reset the connection*. Another device, such as a firewall, might send it to tear a TCP connection. This flag is also used when data is sent to a host and there is no service on the receiving end to answer.
  
- **SYN**: Synchronise flag is used to *initiate a TCP 3-way handshake* and synchronise sequence numbers with the other host. The sequence number should be set randomly during TCP connection establishment.
  
- **FIN**: The sender has *no more data to send*.

