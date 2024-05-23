
Cloud providers and third-party organisations offer a variety of solutions that help organisations achieve their security objectives in the cloud. Organisations may choose to adopt cloud-native controls offered by their cloud service provider, third-party solutions, or a combination of the two.

Controls offered by **cloud service providers** have the advantage of *direct integration with the provider's offerings*, often making them *cost-effective and user-friendly*. 

**Third-party** solutions are often more costly, but they bring the advantage of *integrating with a variety of cloud providers*, facilitating the management of multi-cloud environments.

### Cloud Access Security Brokers

Most organisations use a variety of cloud service providers for different purposes. This is especially true when organisations use *highly specialised SaaS products*. Managing security policies consistently across these services poses a major challenge for cybersecurity analysts.

Cloud access security brokers (**CASBs**) are software tools that serve as *intermediaries between cloud service users and cloud service providers*. This positioning allows them to monitor user activity and enforce policy requirements. CASBs operate using two different approaches:

- **Inline CASB solutions** physically or logically *reside in the connection path between the user and the service*. They may do this through a hardware appliance or an endpoint agent that routes requests through the CASB. This approach requires configuration of the network and/or endpoint devices. It provides the advantage of seeing requests before they are sent to the cloud service, allowing the CASB to *block requests* that violate policy.
  
- **API-based CASB solutions** do not interact directly with the user but rather *interact directly with the cloud provider* through the provider's API. This approach provides direct access to the cloud service and does not require any user device configuration. However, it also *does not allow the CASB to block requests* that violate policy. API-based CASBs are *limited to monitoring user activity and reporting* on or correcting policy violations after the fact.

### Resource Policies

Cloud providers offer resource policies that customers may use to *limit the actions that users of their accounts may take*. Implementing resource policies is a good security practice to limit the damage caused by an accidental command, a compromised account, or a malicious insider.

### Secrets Management

Hardware security modules (**HSMs**) are special-purpose computing devices that manage encryption keys and also perform cryptographic operations in a highly efficient manner. One of their core benefits is that they can *create and manage encryption keys without exposing them to a single human* being, dramatically reducing the likelihood that they will be compromised.

Cloud service providers often use HSMs internally for the management of their own encryption keys and also offer HSM services to their customers as a secure method for managing customer keys without exposing them to the provider.