
The primary responsibility of the hypervisor is **enforcing isolation** between virtual machines. This means that the hypervisor must present each virtual machine with the *illusion of a completely separate physical environment* dedicated for use by that virtual machine.

From an **operational** perspective, isolation ensures that virtual machines do not interfere with each other's operations. From a **security** perspective, it means that virtual machines are not able to access or alter information or resources assigned to another virtual machine.

There are two primary types of hypervisors:

- **Type I hypervisors**, also known as bare metal hypervisors, operate *directly on top of the underlying hardware*. The hypervisor then supports guest operating systems for each virtual machine. This is the model most commonly used in datacenter virtualisation because it is highly efficient.

![[Type I Hypervisor.png]]

- **Type II hypervisors** run as an application on *top of an existing operating system*. This model is commonly used to provide virtualisation environments on personal computers for developers, technologists, and others who have the need to run their own virtual machines. It is less efficient than bare-metal virtualisation because the host operating system introduces a layer of inefficiency that consumes resources.

![[Type II Hyperviser.png]]

