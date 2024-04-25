
Cloud security operations somewhat differ significantly from on-premises environments because *cloud customers must divide responsibilities between one or more service providers and the customers' own cybersecurity teams*.

This type of operating environment is known as the shared responsibility model. The figure below shows the common division of responsibilities in IaaS, PaaS, and SaaS environments, known as the **Responsibility Matrix**.

![[Shared Responsibility Model.png]]

In some cases, this division of responsibility is straightforward. Cloud providers, by their nature, are always responsible for the security of both hardware and the physical datacenter environment. If the customer were handling either of these items, the solution would not fit the definition of cloud computing.

The *differences in responsibility come higher up in the stack* and vary depending on the nature of the cloud service being used. In an **IaaS** environment, the *customer takes over security responsibility for everything that isn't infrastructure*—the operating system, applications, and data that they run in the IaaS environment.

In a **PaaS** solution, the *vendor also takes on responsibility for the operating system*, whereas the customer retains responsibility for the data being placed into the environment and configuring its security. Responsibility for the *application layer is shared between the service provider and the customer*, and the exact division of responsibilities shifts based on the nature of the service. For example, if the PaaS platform provides runtime interpreters for customer code, the cloud provider is responsible for the security of those interpreters.

In an **SaaS** environment, the *provider takes on almost all security responsibility*. The customer retains some shared control over the data that they place in the SaaS environment and the configuration of access controls around that data, but the SaaS provider is being paid to take on the burden of most operational tasks, including cybersecurity.