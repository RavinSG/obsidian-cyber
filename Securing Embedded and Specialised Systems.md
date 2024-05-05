
Security practitioners encounter traditional computers and servers every day, but as smart devices, embedded systems, and other specialised systems continue to be built into everything from appliances, to buildings, to vehicles, and even clothing, the attack surface for organisations is growing in new ways.

### [[Embedded Systems]]

### [[SCADA and ICS]]

### [[Securing the Internet of Things]]


### Communication Considerations

Cellular connectivity, including both existing LTE and other fourth-generation technologies as well as newer 5G network connectivity, can provide high-bandwidth access to embedded systems in many locations where a Wi-Fi network wouldn't work. Since third-party cellular providers are responsible for connectivity, embedded systems that use *cellular connectivity need to be secured so that the cellular network does not pose a threat* to their operation.

Ensuring that they *do not expose vulnerable services or applications via their cellular connections* is critical to their security. Building in protections to prevent network exploits from traversing internal security boundaries such as those between wireless connectivity and local control buses is also a needed design feature.

*Physically securing the* subscriber identity module (*SIM*) built into cellular-enabled devices can be surprisingly important. **SIM cloning attacks** can also allow attackers to present themselves as the embedded system, allowing them to both send and receive information as a trusted system.

Embedded systems may also take advantage of radio frequency protocols specifically designed for them. **Zigbee** is one example of a network protocol that is designed for personal area networks like those found in houses for home automation. They have *limitations on range and how much data they can transfer*, and that *since they are designed for home automation* and similar uses they **do not have strong security models**.

### Security Constraints of Embedded Systems

Embedded systems have a number of constraints that security solutions need to take into account. Since embedded systems may have limited capabilities, differing deployment and management options, and extended lifecycles, they required additional thought to secure.

When you consider security for embedded systems, you should take the following into account:

- The overall computational *power and capacity* of embedded systems is usually *much lower than a traditional* PC or mobile device. That means that the compute power needed for cryptographic processing may not exist, or it may have to be balanced with other needs for CPU cycles. At the same time, *limited memory and storage capacity* mean that there may not be capacity to run additional security tools like a firewall, antimalware tools, or other security tools you're used to including in a design.
  
- Without network connectivity, CPU and memory capacity, and other elements, a*uthentication is also likely to be impossible*. In fact, authenticating to an embedded system may not be desirable due to safety or usability factors.
  
- Embedded systems may be very low cost, but many are *effectively very high cost because they are a component in a larger industrial or specialised device*. So, *simply replacing a vulnerable device can be impossible*, requiring compensating controls or special design decisions to be made to ensure that the devices remain secure and do not create issues for their home organization.

Because of all these limitations, embedded devices may **rely on implied trust**. The implied trust model that goes with physical access for embedded devices makes them a potential vulnerability for organisations, and one that must be reviewed and designed for before they are deployed.

