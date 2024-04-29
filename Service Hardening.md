
One of the fastest ways to decrease the attack surface of a system is to *reduce the number of open ports and services* that it provides.

Port scanners are commonly used to quickly assess which ports are open on systems on a network, allowing security practitioners to identify and prioritise hardening targets.

The easy rule of thumb for hardening is that **only services and ports that must be available to provide necessary services should be open**, and that those ports and services should be limited to only the networks or systems that they need to interact with.

Below are some of the most common ports and services that are open for both Linux and Windows systems.

| Port and Protocol             | Windows          | Linux            |
| ----------------------------- | ---------------- | ---------------- |
| 22/TCP - Secure Shell (SSH)   | Uncommon         | Common           |
| 53/TCP and UDP - DNS          | Common (servers) | Common (servers) |
| 80/TCP - HTTP                 | Common (servers) | Common (servers) |
| 125-139/TCP and UDP - NetBIOS | Common           | Occasional       |
| 389/TCP and UDP - LDAP        | Common (servers) | Common (servers) |
| 443/TCP - HTTPS               | Common (servers) | Common (servers) |
| 3389/TCP and UDP - RDP        | Common           | Uncommon         |

Although blocking a service using a firewall is a viable option, the *best option* for unneeded services is to **disable them entirely**.

In *Windows* you can use the `Services.msc` control to disable or enable services.

Starting and stopping services in *Linux* requires knowing how your Linux distribution handles services. For an Ubuntu Linux system, checking which services are running can be accomplished by using the `service –status-all` command. 

Starting and stopping services can be done in a number of ways, but the service command is an easy method. Issuing the `sudo service [service name]` stop or start commands will start or stop a service simply by using the information provided by the `service--status-all` command to identify the service you want to shut down. 

Permanently stopping services, however, will require you to make other changes. For **Ubuntu**, the `update-rc.d` script is called, whereas **RedHat** systems use `chkconfig`.