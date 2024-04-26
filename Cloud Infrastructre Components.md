
IaaS computing environments provide organisations with access to a wide variety of computing resources, including compute capacity, storage, and networking. These resources are available in a flexible manner and typically may be used immediately upon request.

### Cloud Computing Resources

Computing capacity is one of the primary needs of organisations moving to the [[The Cloud|cloud]]. As they seek to augment or replace the servers running in their own datacenters, they look to the cloud for *virtualised servers* and other means of providing compute capacity. 

#### Virtualisation

Virtual machines are the *basic building block* of compute capacity in the cloud. Organisations may provision servers running most common operating systems with the specific number of **CPU** cores, amount of **RAM**, and **storage** capacity that is necessary to meet business requirements.

Once you've provisioned a virtualised server, you may interact with it in the same manner as you would a server running in your own datacenter.

#### Containerisation

Containers provide **application-level virtualisation**. Instead of creating complex virtual machines that require their own operating systems, containers package applications and allow them to be treated as units of virtualisation that become *portable across operating systems and hardware* platforms.

Organisations implementing containerisation run containerisation platforms, such as Docker, that provide standardised interfaces to operating system resources.

Containerisation platforms share many of the same security considerations as virtualisation platforms. They must **enforce isolation** between containers to prevent operational and security issues.

Specific ways that we can secure containers recommended by NIST include:

- Using container-specific host operating systems, which are built with reduced features to reduce attack surfaces.
- Segmenting containers by risk profile and purpose
- Using container-specific vulnerability management security tools

### Cloud Storage Resources

Infrastructure providers also offer their customers storage resources, both storage that is *coupled with their computing offerings and independent storage* offerings for use in building out other cloud architectures. These storage offerings come in two major categories:

- **Block storage** allocates large volumes of storage for *use by virtual server instance(s)*. These volumes are then formatted as virtual disks by the operating system on those server instances and used as they would a physical drive. *AWS* offers block storage through their *Elastic Block Storage* (EBS) service.
  
- **Object storage** provides customers with the ability to *place files in buckets* and treat each file as an independent entity that may be accessed over the web or through the provider's API. Object storage hides the storage details from the end user, who does not know or care about the underlying disks. The *AWS Simple Storage Service* (S3) is an example of object storage.

As you work with cloud storage, be certain that you keep three key security considerations top-of-mind:

- **Set permissions properly**. Make sure that you pay careful attention to the access policies you place on storage. This is especially true for object storage, where a few wayward clicks can inadvertently publish a sensitive file on the web.
  
- **Consider high availability and durability options**. Cloud providers hide the implementation details from users, but that doesn't mean they are immune from hardware failures. Use the provider's replication capabilities or implement your own to accommodate availability and integrity requirements.
  
- **Use encryption to protect sensitive data**. You may either apply your own encryption to individual files stored in the cloud or use the full-disk encryption options offered by the provider.

### Cloud Networking

Cloud networking follows the same virtualisation model as other cloud infrastructure resources. Cloud consumers are provided access to networking resources to *connect their other infrastructure components* and are able to provision bandwidth as needed to meet their needs.

Cloud networking supports the software-defined networking (**[[Software-defined Networking|SDN]]**) movement by allowing **engineers** to interact with and modify cloud resources through their APIs. Similarly, they provide **cybersecurity professionals** with software-defined visibility (**SDV**) that offers insight into the traffic on their virtual networks.

#### Security Groups

Security professionals use *firewalls on their physical networks* to limit the types of network traffic that are allowed to enter the organisation's secured perimeter. Cloud service providers implement firewalls as well, but they *do not provide customers with direct access to those firewalls*, because doing so would violate the isolation principle by potentially allowing one customer to make changes to the firewall that would impact other customers.

Instead, cloud service providers meet the need for firewalls through the use of security groups that define permissible network traffic. These security groups consist of a set of rules for network traffic that are substantially the same as a firewall ruleset.

![[Security Groups.png]]

>[!note] Note
>*Security groups function at the network layer* of the OSI model, similar to a traditional firewall. Cloud service providers also *offer web application firewall* capabilities that operate at higher levels of the OSI model.

#### Virtual Private Cloud (VPC)

On a physical network, networking and security professionals use virtual LAN (VLAN) technology to achieve segmentation. In cloud environments, virtual private clouds (VPCs) serve the same purpose.

Using VPCs, teams can group systems into subnets and designate those subnets as public or private, depending on whether access to them is permitted from the Internet.

Cloud providers also offer VPC endpoints that allow the *connection of VPCs to each other* using the cloud provider's secure network backbone. **Cloud transit gateways** extend this model even further, allowing the *direct interconnection of cloud VPCs with on-premises VLANs* for hybrid cloud operations.

#### DevOps and Cloud Automation

Traditional approaches to organising and running technology teams focused on building silos of expertise centred on technology roles. In particular, software development and technology operations were often viewed as quite disconnected.

When developers completed testing of a new version of an application, they then handed it off to the technology operations team, who managed the servers and other infrastructure supporting the application.

While separating the development and operations worlds provides technologists with a comfortable working environment, it also brings significant disadvantages, including the following:

- Isolating operations teams from the development process inhibits their understanding of business requirements. 
  
- Isolating developers from operational considerations leads to designs that are wasteful in terms of processor, memory, and network consumption.
  
- Requiring clear hand-offs from development to operations reduces agility and flexibility by requiring a lengthy transition phase.
  
- Increasing the overhead associated with transitions encourages combining many small fixes and enhancements into one major release, increasing the time to requirement satisfaction.

**DevOps** brings together development and operations teams in a unified process where they work together in an agile approach to software development.

**Infrastructure as code** (IaC) is one of the key enabling technologies behind the DevOps movement and is also a crucial advantage of cloud computing services integration. 

**IaC** is the process of automating the provisioning, management, and de-provisioning of infrastructure services through scripted code rather than human intervention. **IaC** is one of the key features of all major IaaS environments, including AWS, Microsoft Azure, and Google Cloud Platform.

IaC takes many forms and may be either a feature offered by a cloud service provider or a functionality enabled by a third-party cloud management platform.

AWS offers a service called **CloudFormation** that allows developers to specify their infrastructure requirements in several formats, including JavaScript Object Notation (*JSON*) and YAML Ain't Markup Language (*YAML*).

Developers can use cloud provider APIs to programmatically provision, configure, modify, and de-provision cloud resources. API integration is particularly helpful in cloud environments that embrace **microservices**, cloud service offerings that *provide very granular functions to other services*, often through a function-as-a-service model. These microservices are designed to communicate with each other in response to events that take place in the environment.

