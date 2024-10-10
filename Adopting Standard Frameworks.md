
### NIST Cybersecurity Framework

The National Institute of Standards and Technology (**[[National Institute of Standards and Technology|NIST]]**) is responsible for developing cybersecurity standards across the U.S. federal government. The guidance and standard documents they produce in this process often have *wide applicability across the private sector and are commonly referred to by nongovernmental security analysts* due to the fact that they are available in the public domain and are typically of very high quality.

In 2018, NIST released version 1.1 of a Cybersecurity Framework ([[Cybersecurity Framework|CSF]]) designed to assist organisations attempting to meet one or more of the following five objectives:

- Describe their current cybersecurity posture.
- Describe their target state for cybersecurity.
- Identify and prioritise opportunities for improvement within the context of a continuous and repeatable process.
- Assess progress toward the target state.
- Communicate among internal and external stakeholders about cybersecurity risk.

The NIST framework includes three components:

The Framework [[Cybersecurity Framework#Core|Core]], is a set of *five security functions that* apply across all industries and sectors: **identify**, **protect**, **detect**, **respond**, and **recover**. The framework then divides these functions into categories, subcategories, and informative references. 

The Framework Implementation [[Cybersecurity Framework#Tiers|Tiers]] assess *how an organisation is positioned to meet cybersecurity objectives*. Table below shows the framework implementation tiers and their criteria. This approach is an example of a maturity model that describes the current and desired positioning of an organisation along a continuum of progress. In the case of the NIST maturity model, organisations are assigned to one of four maturity model tiers.

| Tier                                                      | Risk Management Process                                                                                                                                              | Integrated risk management program                                                                                                                                                                                                               | External Participation                                                                                                                                            |
| --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Tier 1: **Partial**<br><br><br>                           | Organisational cybersecurity risk management practices are not formalised, and risk is managed in an ad hoc and sometimes reactive manner.                           | There is limited awareness of cybersecurity risk at the organisational level. The organisation implements cybersecurity risk management on an irregular, case-by-case basis due to varied experience or information gained from outside sources. | The organization does not understand its role in the larger ecosystem with respect to either its dependencies or dependents.                                      |
| Tier 2: **Risk Informed**<br><br><br><br><br><br><br><br> | Risk management practices are approved by management but may not be established as organization-wide policy.                                                         | There is an awareness of cybersecurity risk at the organisational level, but an organisation-wide approach to managing cybersecurity risk has not been established.                                                                              | Generally, the organization understands its role in the larger ecosystem with respect to either its own dependencies or dependents, but not both.                 |
| Tier 3: **Repeatable**<br><br><br>                        | The organisation's risk management practices are formally approved and expressed as policy.                                                                          | There is an organisation-wide approach to manage cybersecurity risk.                                                                                                                                                                             | The organization understands its role, dependencies, and dependents in the larger ecosystem and may contribute to the community's broader understanding of risks. |
| Tier 4: **Adaptive**                                      | The organization adapts its cybersecurity practices based on previous and current cybersecurity activities, including lessons learned and predictive indicators.<br> | There is an organization-wide approach to managing cybersecurity risk that uses risk-informed policies, processes, and procedures to address potential cybersecurity events.                                                                     | The organization understands its role, dependencies, and dependents in the larger ecosystem and contributes to the community's broader understanding of risks.    |

Framework [[Cybersecurity Framework#Profiles|Profiles]] describes how a specific organisation might approach the security functions covered by the Framework Core. An organisation might use a framework profile to describe its current state and then a separate profile to describe its desired future state.

### NIST Risk Management Framework

In addition to the CSF, NIST publishes a Risk Management Framework ([[Risk Management Framework|RMF]]). The RMF is a mandatory standard for federal agencies that provides a formalised process that federal agencies must follow to select, implement, and assess risk-based security and privacy controls.

### ISO Standards

The International Organisation for Standardisation (ISO) publishes a series of standards that offer best practices for cybersecurity and privacy. You should be familiar with four specific ISO standards: **ISO 27001**, **ISO 27002**, **ISO 27701**, and **ISO 31000**.

#### ISO 27001

ISO 27001 is a standard document titled “*Information security management systems*.” This standard includes **control objectives** covering 14 categories:

- Information security policies
- Organisation of information security
- Human resource security
- Asset management
- Access control
- Cryptography
- Physical and environmental security
- Operations security
- Communications security
- System acquisition, development, and maintenance
- Supplier relationships
- Information security incident management
- Information security aspects of business continuity management
- Compliance with internal requirements, such as policies, and with external requirements, such as laws

The ISO 27001 standard was once the most commonly used information security standard, but it is *declining in popularity outside of highly regulated industries that require ISO compliance*. Organisations in those industries may choose to formally adopt ISO 27001 and pursue certification programs, where an external assessor validates their compliance with the standard and certifies them as operating in accordance with ISO 27001.

#### ISO 27002

The ISO 27002 standard goes beyond control objectives and **describes the actual controls that an organisation may implement** to meet cybersecurity objectives. ISO designed this supplementary document for organisations that wish to

- Select information security controls
- Implement information security controls
- Develop information security management guidelines

#### ISO 27701

Whereas ISO 27001 and ISO 27002 focus on cybersecurity controls, ISO 27701 contains standard guidance for managing **privacy controls**. ISO views this document as an extension to their ISO 27001 and ISO 27002 security standards.

#### ISO 31000

ISO 31000 provides guidelines for **risk management programs**. This document is not specific to cybersecurity or privacy but covers risk management in a general way so that it may be applied to any risk.

### Benchmarks and Secure Configuration Guides

The NIST and ISO frameworks are high-level descriptions of cybersecurity and risk management best practices. They don't offer practical guidance on actually implementing security controls. However, government agencies, vendors, and industry groups publish a variety of benchmarks and secure configuration guides that help organisations understand how they can securely operate commonly used platforms, including operating systems, web servers, application servers, and network infrastructure devices.

These benchmarks and configuration guides get down into the nitty-gritty details of securely operating commonly used systems. For example, the figure below shows an excerpt from a security configuration benchmark for Windows Server 2022.

![[Security Configuration Benchmark.png]]

The excerpt shown in the above figure comes from the Center for Internet Security, an industry organisation that publishes hundreds of benchmarks for commonly used platforms. 