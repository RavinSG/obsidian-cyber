
Keeping an endpoint secure while it is running starts as it boots up. If untrusted or *malicious components are inserted into the boot process, the system cannot be trusted*. Security practitioners in high-security environments need a means of ensuring that the entire boot process is provably secure.

Fortunately, modern *Unified Extensible Firmware Interface* (**UEFI**) firmware (the replacement for the traditional *Basic Input/Output System* (**BIOS**)) can leverage two different techniques to ensure that the system is secure.

**Secure boot** ensures that the system boots using only software that the original equipment manufacturer (**OEM**) trusts. To perform a secure boot operation, the system must have a signature database listing the secure signatures of trusted software and firmware for the boot process.

![[UEFI secure boot.png]]

The second security feature intended to help prevent boot-level malware is **measured boot**. These boot processes measure each component, starting with the firmware and ending with the boot start drivers. *Measured boot does not validate against a known good list* of signatures before booting; instead, it relies on the UEFI firmware to hash the firmware, bootloader, drivers, and anything else that is part of the boot process. 

The data gathered is stored in the Trusted Platform Module (**TPM**), and the logs can be validated remotely to let security administrators know the boot state of the system. This *boot attestation* process allows comparison against known good states, and administrators can take action if the measured boot shows a difference from the accepted or secure known state.

In both cases, boot integrity begins with the **hardware root of trust**. One common implementation of a hardware root of trust is the *TPM chip* built into many computers. TPM chips are frequently used to provide built-in encryption, and they provide three major functions.

- **Remote attestation**, allowing hardware and software configurations to be verified 
- **Binding**, which encrypts data 
- **Sealing**, which encrypts data and sets requirements for the state of the TPM chip before decryption

TPM chips are one common solution; others include serial numbers that cannot be modified or cloned, and *physically unclonable functions* (**PUFs**), which are unique to the specific hardware device that provide a unique identifier or digital fingerprint for the device.

>[!hint] Physically Unclonable Function
>A physically unclonable function is based on the unique features of a microprocessor that are created when it is manufactured and is not intentionally created or replicated.

Similar techniques are used for **Apple's Secure Enclave**, a dedicated secure element that is built into Apples's system on chip (SoC) modules. They provide hardware key management, which is isolated from the CPU, protecting keys throughput their lifecycle and usage.

>[!note] Protecting the Keys to the Kingdom
>A related technology is hardware security modules (HSMs). Hardware security modules are typically external devices or plug-in cards used to create, store, and manage digital keys for cryptographic functions and authentication, as well as to offload cryptographic processing. *HSMs are often used in high-security environments* and are normally certified to meet standards like Federal Information Processing Standards (FIPS) 140 or Common Criteria standards (ISO/IEC 15408).
>
>Cryptographic key management systems (KMS) are used to store keys and certificates as well as manage the, centrally.  Cloud providers frequently provide KMS as a service for their environments as part of their offering.



