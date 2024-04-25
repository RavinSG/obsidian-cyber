
Cloud deployment models describe how a cloud service is delivered to customers and whether the resources used to offer services to one customer are shared with other customers.

### Public Cloud

Public cloud service providers deploy infrastructure and then make it *accessible to any customer* who wishes to take advantage of it in a **multitenant** model. A single customer may be running workloads on servers spread throughout one or more datacenters, and those servers may be running workloads for many different customers simultaneously.

The public cloud *supports all cloud service models*. The key distinction is that those services do not run on infrastructure dedicated to a single customer but rather on infrastructure that is available to the general public. AWS, Microsoft Azure, and Google Compute Platform all use the public cloud model.

### Private Cloud

The term private cloud is used to describe any cloud infrastructure *that is provisioned for use by a single customer*. This infrastructure may be built and managed by the organization that will be using the infrastructure, or it may be built and managed by a third party. 

The key distinction here is that **only one customer uses the environment**. For this reason, private cloud services tend to have excess unused capacity to support peak demand and, as a result, are *not as cost efficient as public cloud* services.

### Community Cloud

A community cloud service shares characteristics of both the public and private models. Community cloud services do *run in a multitenant environment, but the tenants are limited to members* of a specifically designed community. Community membership is normally defined based on shared mission, similar security and compliance requirements, or other commonalities.

The **HathiTrust** digital library, is an example of community cloud in action. Academic research libraries joined together to form a consortium that provides access to their collections of books. Students and faculty at HathiTrust member institutions may log into the community cloud service to access resources.

### Hybrid Cloud

Hybrid cloud is a catch-all term used to describe cloud deployments that *blend public, private, and/or community cloud services together*. It is **not simply purchasing both** public and private cloud services and using them together. 

Hybrid clouds require the use of technology that **unifies the different cloud offerings** into a single coherent platform.

For example, a firm might operate their own private cloud for the majority of their workloads and then leverage public cloud capacity when demand exceeds the capacity of their private cloud infrastructure. This approach is known as *public cloud bursting*.

**AWS Outposts** are examples of hybrid cloud computing. Customers of this service receive a rack of computing equipment that they install in their own datacenters. The equipment in the rack is maintained by AWS but provisioned by the customer in the same manner as their AWS public cloud resources. 

This approach qualifies as hybrid cloud because customers can manage both their on-premises AWS Outposts private cloud deployment and their public cloud AWS services through the same management platform.