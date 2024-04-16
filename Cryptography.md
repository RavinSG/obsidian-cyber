## Goals of Cryptography

Security practitioners use cryptographic systems to meet *four fundamental goals*: **confidentiality**, **integrity**, **authentication**, and **nonrepudiation**. Achieving each of these goals requires the satisfaction of a number of design requirements, and not all crypto-systems are intended to achieve all four goals.

### Confidentiality

Confidentiality ensures that data remains private in three different situations: when it is *at rest*, when it is *in transit*, and when it is *in use*.

Confidentiality is perhaps the most *widely cited goal* of cryptosystems—the preservation of secrecy for stored information or for communications between individuals and groups. Two main types of cryptosystems enforce confidentiality.

- Symmetric cryptosystems
- Asymmetric cryptosystems

**Obfuscation** is a concept closely related to confidentiality. It is the practice of making it intentionally difficult for humans to understand how code works. This technique is often used to hide the inner workings of software, particularly when it contains sensitive intellectual property.

### Integrity

Integrity ensures that data is not altered without authorisation. If integrity mechanisms are in place, the *recipient* of a message *can be certain* that the message *received is identical* to the message that was sent. Similarly, integrity checks can ensure that stored data was not altered between the time it was created and the time it was accessed.

Integrity controls protect against all forms of alteration, including intentional alteration, intentional deletion, and unintentional alteration.

Message integrity is enforced through the use of encrypted message digests, known as **digital signatures**, created upon transmission of a message.

### Authentication

Authentication *verifies the claimed identity* of system users and is a major function of cryptosystems. For example, suppose that Bob wants to establish a communications session with Alice and they are both participants in a shared secret communications system. Alice might use a **challenge-response authentication** technique to ensure that Bob is who he claims to be.

### Non-repudiation

Nonrepudiation *provides assurance* to the recipient that the *message was originated by the sender* and not someone masquerading as the sender. It also *prevents the sender* from *claiming* that *they never sent the message* in the first place (also known as repudiating the message).

Nonrepudiation is offered only by public key, or asymmetric, cryptosystems.

[[Cipher]]

### [[Emerging Issues in Cryptography]]