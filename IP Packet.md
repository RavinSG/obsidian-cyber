
### Format of an IPv4 Packet

- An IPv4 header format is determined by the IPv4 protocol and includes the IP routing information that devices use to direct the packet. The size of the IPv4 header ranges from *20 to 60 bytes*. The *first 20 bytes are a fixed set of information* containing data such as the source and destination IP address, header length, and total length of the packet. The last set of bytes can range from 0 to 40 and consists of the options field.

- The length of the data section of an IPv4 packet can vary greatly in size. However, the **maximum possible size of an IPv4 packet is 65,535 bytes**. It contains the message being transferred over the internet, like website information or email text.

![[IPv4 Header.png]]

There are 13 fields within the header of an IPv4 packet:

- **Version (VER)**: This 4 bit component tells receiving devices what protocol the packet is using. The packet used in the illustration above is an IPv4 packet.

- **IP Header Length (HLEN or IHL)**: HLEN is the packet’s header length. This value indicates where the packet header ends and the data segment begins. 

 - **Type of Service (ToS)**: Routers prioritise packets for delivery to maintain quality of service on the network. The ToS field provides the router with this information.

 - **Total Length**: This field communicates the total length of the entire IP packet, including the header and data. The maximum size of an IPv4 packet is 65,535 bytes.

 - **Identification**: For IPv4 packets that are larger than 65, 535 bytes, the packets are divided, or fragmented, into smaller IP packets. The identification field provides a unique identifier for all the fragments of the original IP packet so that they can be reassembled once they reach their destination. 

 - **Flags**: This field provides the routing device with more information about whether the original packet has been fragmented and if there are more fragments in transit.

 - **Fragmentation Offset**: The fragment offset field tells routing devices where in the original packet the fragment belongs.

 - **Time to Live (TTL)**: TTL prevents data packets from being forwarded by routers indefinitely. It contains a counter that is set by the source. The counter is decremented by one as it passes through each router along its path. When the TTL counter reaches zero, the router currently holding the packet will discard the packet and return an ICMP Time Exceeded error message to the sender. 

 - **Protocol**: The protocol field tells the receiving device which protocol will be used for the data portion of the packet.

 - **Header Checksum**: The header checksum field contains a checksum that can be used to detect corruption of the IP header in transit. Corrupted packets are discarded.

 - **Source IP Address**: The source IP address is the IPv4 address of the sending device.

 - **Destination IP Address**: The destination IP address is the IPv4 address of the destination device.

 - **Options**: The options field allows for security options to be applied to the packet if the HLEN value is greater than five. The field communicates these options to the routing devices.

### Difference between IPv4 and IPv6

Some of the key differences between IPv4 and IPv6 include the length and the format of the addresses. **IPv4** addresses are made up of four decimal numbers separated by periods, each number ranging from 0 to 255. Together the numbers span 4 bytes, and allow for up to **4.3 billion** possible addresses.

**IPv6** addresses are made of eight hexadecimal numbers separated by colons, each number consisting of up to four hexadecimal digits. Together, all numbers span 16 bytes, and allow for up to **340 undecillion** addresses! ($340 \times 10^{36}$)

There are also some differences in the layout of an IPv6 packet header. The *IPv6 header format is much simpler than IPv4*. For example, the IPv4 Header includes the IHL, Identification, and Flags fields, whereas the IPv6 does not. The IPv6 header **only introduces the Flow Label field**, where the Flow Label identifies a packet as requiring special handling by other IPv6 routers. 

![[IPv4 vs. IPv6.png]]

There are some important *security differences* between IPv4 and IPv6. IPv6 offers more efficient routing and *eliminates private address collisions* that can occur on IPv4 when two devices on the same network are attempting to use the same address. 