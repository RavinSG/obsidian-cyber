---
aliases:
  - Idle/Zombie Scan
---
[[Spoofing and Decoy|Spoofing]] the source IP address can be a great approach to scanning stealthily. However, *spoofing will only work in specific network setups*. It requires you to be in a position where you can monitor the traffic. Considering these limitations, spoofing your IP address can have little use; however, we can give it an upgrade with the idle scan.

The idle scan, or zombie scan, requires an *idle system connected to the network* that you can communicate with. Practically, Nmap will make each probe *appear as if coming from the idle* (zombie) host, then it will *check for indicators* whether the idle (zombie) host received any response to the spoofed probe. 

This is accomplished by **checking the IP identification** (IP ID) value in the IP header. You can run an idle scan using `nmap -sI ZOMBIE_IP 10.10.4.187`, where `ZOMBIE_IP` is the IP address of the idle host (zombie).

The idle (zombie) scan requires the following three steps to discover whether a port is open:

1) Trigger the idle host to respond so that you can *record the current IP ID* on the idle host.
2) *Send a SYN* packet to a TCP port on the target. The packet should be *spoofed* to appear as if it was coming from the idle host (zombie) IP address.
3) Trigger the idle machine again to respond so that you can *compare the new IP ID* with the one received earlier.

First the attacker system probing an idle machine, a multi-function printer. By sending a SYN/ACK, it responds with an RST packet containing its newly incremented IP ID.

![[Record Idle IP ID.png]]

The attacker will *send a SYN packet* to the TCP port they want to check on *the target* machine in the next step. However, this packet will use the idle host (zombie) IP address as the source.  **Three scenarios** would arise. 

In the **first scenario**, shown in the figure below, the **TCP port is closed**; therefore, the target machine responds to the idle host with an **RST packet**. The *idle host does not respond*; hence its **IP ID is not incremented**.

![[Idle Port Closed.png]]

In the **second scenario**, as shown below, the **TCP port is open**, so the target machine responds with a **SYN/ACK** to the idle host (zombie). The *idle host responds* to this unexpected packet with an RST packet, thus **incrementing its IP ID**.

![[Idle Open.png]]

In the **third scenario**, the *target* machine *does not respond* at all due to *firewall rules*. This lack of response will lead to the same result as with the closed port; the idle host **won’t increase the IP ID**.

For the final step, the attacker sends *another SYN/ACK to the idle host*. The idle host responds with an RST packet, incrementing the IP ID by one again. The attacker needs to *compare the IP ID* of the RST packet received in the first step with the IP ID of the RST packet received in this third step. 

If the **difference is 1**, it means the port on the target machine was **closed or filtered**. 

However, if the **difference is 2**, it means that the port on the target was **open**.

It is worth repeating that this scan is called an idle scan because *choosing an idle host is indispensable* for the accuracy of the scan. If the “idle host” is busy, all the returned IP IDs would be useless.