---
aliases:
  - SNMP
tags:
  - Protocol
---
Simple Network Management Protocol (**SNMP**) is a network protocol used for monitoring and managing devices on a network. SNMP objects like network switches, routers, and other devices are listed in a management information base (**MIB**) and are queried for SNMP information.

When a device configured to use SNMP *encounters an error*, it sends a message known as a **SNMP trap**. Unlike other SNMP traffic, SNMP traps are *sent to a SNMP manager* from SN.MP agents on devices when the device needs to notify the manager. SNMP traps include information about what occurred so that the manager can take appropriate action.

The base set of SMP traps are *coldStart*, *warmStart*, *linkDown*, *linkUp*, *authentication-Failure*, and *egpNeighborLoss*. Additional custom traps can be configured and are often created by vendors specifically for their devices to provide information.

SNMP can *reset a password* on a network device or *change its baseline configuration*. It can also send requests to network devices for a report on how much of the network’s bandwidth is being used up. 

In the TCP/IP model, SNMP occurs at the [[Application Layer]]
