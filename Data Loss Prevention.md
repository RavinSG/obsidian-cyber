---
aliases:
  - DLP
---
Data loss prevention (**DLP**) systems help organisations *enforce information handling policies and procedures* to prevent data loss and theft. They **search systems** for stores of sensitive information that might be unsecured and **monitor network traffic** for potential attempts to remove sensitive information from the organization. They can act quickly to block the transmission before damage is done and alert administrators to the attempted breach.

DLP systems work in two different environments:

### Agent-based DLP

**Agent-based DLP** uses software agents installed on systems that *search those systems for the presence of sensitive information*. These searches often turn up Social Security numbers, credit card numbers, and other sensitive information in the most unlikely places! Detecting the presence of stored sensitive information allows security professionals to take prompt action to either remove it or secure it with encryption.

Host-based DLP can *also monitor system configuration and user actions*, blocking undesirable actions. For example, some organisations use host-based DLP to block users from accessing USB-based removable media devices that they might use to carry information out of the organisation's secure environment.

### Agentless (network-based) DLP

**Agentless (network-based) DLP** systems are dedicated devices that *sit on the network and monitor outbound network traffic*, watching for any transmissions that contain unencrypted sensitive information. They can then block those transmissions, preventing the unsecured loss of sensitive information.

DLP systems may simply block traffic that violates the organisation's policy, or in some cases, they may automatically apply encryption to the content. This automatic encryption is commonly used with DLP systems that focus on email.


DLP systems also have two mechanisms of action:

- **Pattern matching**, where they watch for the telltale signs of sensitive information. For example, if they see a number that is formatted like a credit card or Social Security number, they can automatically trigger on that. Similarly, they may contain a database of sensitive terms, such as “Top Secret” or “Business Confidential,” and trigger when they see those terms in a transmission.

- **Watermarking**, where systems or administrators apply electronic tags to sensitive documents and then the DLP system can monitor systems and networks for unencrypted content containing those tags. Watermarking technology is also commonly used in digital rights management (DRM) solutions that enforce copyright and data ownership restrictions.

[[Data Protection]]