
The internet layer, sometimes referred to as the *network layer*, is responsible for ensuring the delivery to the destination host, which potentially resides on a **different network**. 

It ensures IP addresses are attached to data packets to indicate the location of the sender and receiver. The internet layer also determines which protocol is responsible for delivering the data packets and ensures the delivery to the destination host. Here are some of the common protocols that operate at the internet layer:

- **Internet Protocol (IP)**: IP sends the data packets to the correct destination and relies on the *Transmission Control Protocol/User Datagram Protocol* (TCP/UDP) to deliver them to the corresponding service. IP packets allow communication between two networks. They are routed from the sending network to the receiving network. The TCP/UDP retransmits any data that is lost or corrupt.

- **Internet Control Message Protocol (ICMP)**: The ICMP shares error information and status updates of data packets. This is useful for *detecting and troubleshooting network errors*. The ICMP reports information about packets that were dropped or that disappeared in transit, issues with network connectivity, and packets redirected to other routers.