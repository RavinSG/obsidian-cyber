
[[The Cloud|Cloud computing]] providers, as well as most other modern datacenter operators, make extensive use of virtualisation technology to *allow multiple guest systems to share the same underlying hardware*. In a virtualised datacenter, the virtual host hardware runs a special operating system known as a [[Hypervisor]] that mediates access to the underlying hardware resources.

Virtual machines then *run on top of this virtual infrastructure* provided by the hypervisor, running standard operating systems such as Windows and Linux variants. 

The virtual machines may not be aware that they are running in a virtualised environment because the *hypervisor tricks them into thinking that they have normal access* to the underlying hardware when, in reality, that hardware is shared with other systems.