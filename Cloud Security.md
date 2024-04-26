
The cloud brings tremendous operational and financial advantages to organisations, but those advantages also come with new security issues that arise in cloud  environments.

### Availability

One of the major advantages of the cloud is that cloud providers may *operate in many different geographic regions*, and they often provide simple mechanisms for backing up data across those regions and/or operating in a high availability mode across diverse zones. 

For example, a company operating a web server cluster in the cloud may choose to place servers on each major continent to serve customers in those regions and also to provide geographic diversity in the event of a large-scale issue in a particular geographic region.

### Data Sovereignty

The distributed nature of cloud computing involves the use of *geographically distant facilities to achieve high availability* and to place content in close proximity to users. This may mean that a customer's data is stored and processed in datacenters across many different countries, either with or without explicit notification. Unless customers understand how their data is stored, this could introduce legal concerns.

**Data sovereignty** is a principle that states that *data is subject to the legal restrictions of any jurisdiction where it is collected, stored, or processed*. Under this principle, a customer might wind up subject to the legal requirements of a jurisdiction where they have no involvement other than the fact that one of their cloud providers operates a datacenter within that jurisdiction.

Security professionals responsible for managing cloud services should be certain that they *understand how their data is stored, processed, and transmitted across jurisdictions*. They may also choose to encrypt data using keys that remain outside the providers control to ensure that they maintain sole control over their data.

### Virtualisation Security

Virtual machine escape vulnerabilities are the most serious issue that can exist in a virtualised environment, particularly when a virtual host runs systems of differing security levels.

In an escape attack, the attacker has access to a single virtual host and then manages to leverage that access to intrude upon the resources assigned to a different virtual machine. Escape attacks allow a process running on the virtual machine to “escape” those hypervisor restrictions.

**Virtual machine sprawl** occurs when IaaS users create virtual service instances and then forget about them or abandon them, leaving them to accrue costs and accumulate security issues over time. Organisations should maintain instance awareness to avoid VM sprawl issues.

**Resource reuse** occurs when cloud providers take hardware resources that were originally assigned to one customer and reassign them to another. If the data was not properly removed from that hardware, the new customer may inadvertently gain access to data belonging to another customer.

### Application Security

Cloud applications depend heavily upon the use of application programming interfaces (APIs) to provide service integration and interoperability. In addition to implementing the secure coding practices, security analysts are responsible for API-based applications should implement *API inspection technology that scrutinises API requests* for security issues.

**Secure web gateways** (SWG) also provide a layer of application security for cloud-dependent organisations. SWGs *monitor web requests made by internal users* and evaluate them against the organisation's security policy, blocking requests that run afoul of these requirements.

### Governance and Auditing of Third-Party Vendors

Technology governance efforts guide the work of IT organisations and *ensure that they are consistent with organisational strategy and policy*. These efforts also should guide the establishment and maintenance of cloud vendor relationships. Cloud governance efforts assist with the following:

- Vetting vendors being considered for cloud partnerships 
- Managing vendor relationships and monitoring for early warning signs of vendor stability issues
- Overseeing an organisation's portfolio of cloud activities

