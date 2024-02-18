
A **firewall** is a network security device that monitors traffic to or from your network. Firewalls can also restrict specific incoming and outgoing network traffic. 

The organization configures the security rules. Firewalls often reside between the secured and controlled internal network and the untrusted network resources outside the organization, such as the internet.

- **Hardware firewall** - A hardware firewall is considered the most basic way to defend against threats to a network. A hardware firewall inspects each data packet before it's allowed to enter the network. 

- **Software firewall** - A software firewall performs the same functions as a hardware firewall, but it's not a physical device. Instead, it's a software program installed on a computer or on a server. If the software firewall is installed on a computer, it will analyse all the traffic received by that computer. If the software firewall is installed on a server, it will protect all the devices connected to the server. A software firewall typically costs less than purchasing a separate physical device, and it doesn't take up any extra space. But because it is a software program, it will add some processing burden to the individual devices.

- **Cloud-based firewall** - Cloud service providers offer firewalls as a service, or *FaaS*, for organisations. Cloud-based firewalls are *software firewalls hosted by a cloud service* provider. Organisations can configure the firewall rules on the cloud service provider's interface, and the firewall will perform security operations on all incoming traffic before it reaches the organisation’s onsite network. Cloud-based firewalls also protect any assets or processes that an organization might be using in the cloud.

### Types of firewalls

- **Stateful** - Stateful refers to a class of firewall that keeps track of information passing through it and proactively filters out threats. A stateful firewall analyses network traffic for characteristics and behaviour that appear suspicious and stops them from entering the network

- **Stateless** - Stateless refers to a class of firewall that operates based on *predefined rules* and does not keep track of information from data packets. A stateless firewall only acts according to preconfigured rules set by the firewall administrator. The rules programmed by the firewall administrator tell the device what to accept and what to reject. A stateless firewall doesn't store analysed information. It also doesn't discover suspicious trends like a stateful firewall does. For this reason, stateless firewalls are considered less secure than stateful firewalls.

- **Next generation firewall** (NGFW) - provides even more security than a stateful firewall. Not only does an NGFW provide stateful inspection of incoming and outgoing traffic, but it also performs more *in-depth security functions* like *deep packet inspection* and *intrusion protection*. Some NGFWs connect to cloud-based threat intelligence services so they can quickly update to protect against emerging cyber threats.