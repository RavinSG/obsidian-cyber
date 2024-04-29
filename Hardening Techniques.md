
Ensuring that a system has booted securely is just the first step in keeping it secure. Hardening endpoints and other systems relies on a variety of techniques that protect the system and the software that runs on it.

### Hardening

Hardening a system or application involves changing settings on the system to *increase its overall level of security and reduce its vulnerability* to attack. The concept of a system's attack surface, or the places where it could be attacked, is important when performing system hardening. 

**Hardening tools and scripts** are a common way to perform basic hardening processes on systems, and organisations like the Center for Internet Security (CIS), found [here](http://www.cisecurity.org/cis-benchmarks), and the National Institute of Standards and Technology ([[National Institute of Standards and Technology|NIST]]), found [here](http://ncp.nist.gov/repository), provide hardening guides for operating systems, browsers, and a variety of other hardening
targets.

#### [[Service Hardening]]

#### [[Network Hardening]]

A common technique used in hardening networks is the use of *VLANs to segment* different trust levels, user groups, or systems. *Placing IoT devices on a seperate, protected VLAN* with appropriate access controls or dedicated network security protections can help to ensure that frequently vulnerable devices are more protected. Using a VLAN for guest networks or to isolate VoIP phones from workstations is also a common practice.

#### Default Passwords

Changing default passwords is a common hardening practice and should be a default practice for any organisation. Since default passwords are documented and publicly available, any use of default passwords creates significant risk for organisations. There are databases of default passwords easily available, including the version provided by [CIRT.net](https://cirt.net/passwords)

#### Removing Unnecessary Software

Another key practice in hardening efforts for many systems and devices is removing unnecessary software. While disabling services, ports, and protocols can be helpful, removing software that isn't needed removes the *potential for a disabled tool to be reenabled*. It also reduces the amount of patching and monitoring that will be required for the system.

Organisations often build their own system images and reinstall a fresh operating system image without unwanted software installed to simplify the process while also allowing them to deploy the software that they do want and need for business purposes.

