Comprehensive vulnerability management programs provide the ability to conduct scans from a variety of scan perspectives. Each scan perspective conducts the scan from a different location on the network, providing a different view into vulnerabilities. 

- **External scan** is run from the Internet, giving administrators a view of what an attacker located outside the organization would see as potential vulnerabilities.

- **Internal scans** might run from a scanner on the general corporate network, providing the view that a malicious insider might encounter.

- Scanners located **inside the datacenter** and agents located on the servers offer the most accurate view of the real state of the server by showing vulnerabilities that might be blocked by other security controls on the network.

Controls that might affect scan results include the following:

- Firewall settings
- Network segmentation
- Intrusion detection systems (IDSs)
- Intrusion prevention systems (IPSs)

> [!tip] Tip
> The internal and external scans required by PCI DSS are a good example of scans performed from different perspectives. The organization may conduct its own internal scans but must *supplement them with external scans conducted by an approved scanning vendor*.

