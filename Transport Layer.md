
The transport layer is responsible for delivering data between two systems or networks and includes protocols to control the flow of traffic across a network. *TCP* and *UDP* are the two transport protocols that occur at this layer. 

### Transmission Control Protocol 

The [[Transmission Control Protocol]] (**TCP**) is an internet communication protocol that allows two devices to form a connection and stream data. It ensures that data is *reliably transmitted* to the destination service. TCP contains the port number of the intended destination service, which resides in the TCP header of a TCP/IP packet.

### User Datagram Protocol 

The [[User Datagram Protocol]] (**UDP**) is a connectionless protocol that does not establish a connection between devices before transmissions. It is used by applications that are *not concerned with the reliability* of the transmission. 

Data sent over UDP is not tracked as extensively as data sent using TCP. Because UDP does not establish network connections, it is used mostly for performance sensitive applications that operate in real time, such as video streaming.