
Hardening operating systems relies on changing settings to match the desired security stance for a given system. Popular benchmarks and configuration standards can be used as a base and modified to an organisation's needs, allowing tools like the **CIS** benchmark to be used throughout an organization. 

### Hardening the Windows Registry

The Windows registry is the core of how Windows tracks what is going on. The registry is thus an *important target for attackers*, who can use it to automatically start programs, gather information, or otherwise take malicious action on a target machine. Hardening the Windows registry involves *configuring permissions for the registry, disallowing remote registry access* if it isn't required for a specific need, and *limiting access to registry tools like regedit* so that attackers who do gain access to a system will be less likely to be able to change or view the registry.

### Windows Group Policy and Hardening

Windows Group Policy provides Windows systems and domains with the ability to control settings through **Group Policy Object** ([[Group Policies|GPOs]]). GPOs can define a wide range of options from disabling guest accounts and setting password minimum lengths to restricting software installations. 

Microsoft provides the Security Compliance Toolkit (SCT), which is a set of tools that work with Microsoft's security configuration baselines for Windows and other Microsoft applications.

### Hardening Linux: SELinux

SELinux is a Linux kernel-based security module that provides additional security capabilities and options on top of existing Linux distributions. Among those capabilities are **mandatory access control** (MAC) that can be enforced at the user, file, system service, and network layer as part of its support for least privilege-based security implementations.

### [[Configuration, Standards, and Schemas]]

### Encryption

Keeping the contents of disks secure protects data in the event that a system or disk is lost or stolen. That's where disk encryption comes in. **Full-disk encryption** (FDE) encrypts the disk and *requires that the bootloader or a hardware device provide a decryption key* and software or hardware to decrypt the drive for use.

One of the most common implementations of this type of encryption is **transparent encryption** (sometimes called on-the-fly, or real-time, encryption). Transparent encryption implementations are largely invisible to the user, with the drive appearing to be unencrypted during use. This also means that the *simplest attack* against a system that uses transparent FDE is to *gain access to the system while the drive is unlocked*.

**Volume encryption** (sometimes called filesystem-level encryption) *protects specific volumes of the drive*, allowing *different trust levels* and additional security beyond that provided by encrypting the entire disk with a single key. File and folder encryption methods can also be used to protect specific data, again allowing for different trust levels, as well as transfer of data in secure ways.

Full-disk encryption can be implemented at the hardware level using a **self-encrypting drive** (SED). Self-encrypting drives *implement encryption capabilities in their hardware and firmware*. Systems equipped with a self-encrypting drive *require a key to boot from the drive*, which may be entered manually or provided by a hardware token or device. Since this is a form of full-disk encryption, the same sort of attack methods work—simply find a logged-in system or one that is in sleep mode.

The significant advantage of full-disk encryption is that a lost or stolen system with a fully encrypted drive can often be **handled as a loss of the system**, instead of a loss or breach of the data that system contained.

