Network segmentation divides a network into physical or logical groupings that are frequently based on trust boundaries, functional requirements, or other reasons that help an organisation apply controls or assist with functionality.

One of the most commonly used concepts is the use of [[Virtual Local Area Network|VLANs]]. A VLAN sets up a broadcast domain that is segmented at the datalink layer. Switches or other devices are used to create a VLAN using VLAN tags, *allowing different ports across multiple network devices like switches to be all part of the same VLAN* without other systems plugged into those switches being the same broadcast domain.

A number of network design concepts describe specific implementations of network segmentation:

- Screened subnets (often called [[Demilitarised Zones|DMZs]]), are network zones that *contain systems that are exposed to less trusted areas*. Screened subnets are commonly used to contain web servers or other Internet-facing devices but can also describe internal purposes where trust levels are different.
  
- *Intranets are internal networks* set up to provide information to employees or other members of an organization, and they are typically protected from external access.
  
- *Extranets* are networks that are set up for external *access, typically by partners or customers* rather than the public at large.
  
Although many network designs used to presume that threats would come from outside the security boundaries used to define network segments, the **core concept of [[Zero Trust]] networks is that nobody is trusted**, regardless of whether they are an internal or an external person or system. Therefore, Zero Trust networks should include security between systems as well as at security boundaries.