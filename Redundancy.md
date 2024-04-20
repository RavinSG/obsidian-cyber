
Common elements in designs for redundancy include the following:

- *Geographic dispersal* of systems ensures that a single disaster, attack, or failure cannot disable or destroy them. For data centres and other facilities, a common rule of thumb is to place data centres at least 90 miles apart, preventing most common natural disasters from disabling both (or more!) data centres. This also helps ensure that facilities will not be impacted by issues with the power grid, network connectivity, and other similar issues.
  
- *Separation of servers* and other devices in data centres is also commonly used to avoid a single rack being a point of failure. Thus, systems may be placed in two or more racks in case of a single point failure of a power distribution unit (PDU) or even something as simple as a leak that drips down into the rack.
- Use of *multiple network paths* (multipath) solutions ensures that a severed cable or failed device will not cause a loss of connectivity.
- *Redundant network devices*, including multiple routers, security devices like firewalls and intrusion prevention systems, or other security appliances, are also commonly implemented to prevent a single point of failure.
  
	- **Load balancers**, allowing both redundancy and increased ability to handle loads by distributing it to more than one system. Load balancers are also commonly used to allow system upgrades by redirecting traffic.
	  
	- **Clustering** describes groups of computers connected together to perform the same task. A cluster of computers might provide the web front-end of an application or serve as worker nodes in a supercomputer.
	  
- *Protection of power*, through the use of uninterruptible power supply (UPS) systems that provide battery or other backup power options for short periods of time; generator systems that are used to provide power for longer outages. Managed power distribution units (PDUs) are also used to provide intelligent power management and remote control of power delivered inside server racks and other environments.
  
- Systems and *storage redundancy* helps ensure that failed disks, servers, or other devices do not cause an outage.
  
- *Platform diversity*, or diversity of technologies is another way to build resilience into an infrastructure. Using different vendors, cryptographic solutions, platforms, and controls can make it more difficult for a single attack or failure to have system- or organisation-wide impacts. There is a real **cost to using different technologies** such as additional training, the potential for issues when integrating disparate systems, and the potential for human error that increases as complexity increases.
