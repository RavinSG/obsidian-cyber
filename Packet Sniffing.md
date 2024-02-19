
**Packet sniffing** is the practice of *capturing and inspecting data packets across a network*. On a private network, data packets are directed to the matching destination device on the network. 

The device’s Network Interface Card (**NIC**) is a piece of hardware that connects the device to a network. The NIC reads the data transmission, and if it contains the device’s MAC address, it accepts the packet and sends it to the device to process the information based on the protocol. This occurs in all standard network operations. 

However, a *NIC can be set to promiscuous mode*, which means that it accepts all traffic on the network, even the packets that aren’t addressed to the NIC’s device. Malicious actors might use [[Network Protocol Analysers|packet sniffers]] like Wireshark to capture the data on a private network and store it for later use. 

They can then use the personal information to their own advantage. Alternatively, they might use the IP and MAC addresses of authorised users of the private network to perform IP spoofing.