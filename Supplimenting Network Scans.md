Basic vulnerability scans run over a network, *probing a system from a distance*. This provides a realistic view of the system's security by simulating what an attacker might see from another network vantage point. However, the *firewalls, intrusion prevention systems, and other security controls* that exist on the path between the scanner and the target server *may affect the scan results*, providing an inaccurate view of the server's security independent of those controls.

Vulnerability scans that run over the network *may detect the possibility* that a vulnerability exists but be *unable to confirm it with confidence*, causing a false positive result that requires time-consuming administrator investigation.

Modern vulnerability management solutions can *supplement these remote scans with trusted information* about server configurations. This information may be gathered in two ways. 

#### Credentialed Scans

First, *administrators can provide the scanner with credentials* that allow the scanner to connect to the target server and retrieve configuration information. This information can then be used to determine whether a vulnerability exists, improving the scan's accuracy over noncredentialed alternatives. For example, if a vulnerability scan detects a potential issue that can be corrected by an operating system update, the credentialed scan can check whether the update is installed on the system before reporting a vulnerability.

> [!tip] Tip
> Credentialed scans typically *only retrieve information* from target servers and do not make changes to the server itself. Therefore, administrators should enforce the principle of least privilege by providing the scanner with a **read-only account** on the server. This reduces the likelihood of a security incident related to the scanner's credentialed access.

#### Agent-based Scanning

In addition to credentialed scanning, some scanners supplement the traditional server-based scanning approach to vulnerability scanning with a complementary agent-based scanning approach. In this approach, *administrators install small software agents on each target server*. These agents conduct scans of the server configuration, providing an “inside-out” vulnerability scan, and then report information back to the vulnerability management platform for analysis and reporting.

> [!tip] Tip
> System administrators are typically wary of installing agents on the servers that they manage for fear that the agent will cause performance or stability issues. If you choose to use an agent-based approach to scanning, you should approach this concept *conservatively, beginning with a small pilot deployment* that builds confidence in the agent before proceeding with a more widespread deployment.
