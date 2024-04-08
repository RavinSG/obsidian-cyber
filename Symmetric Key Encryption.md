
Symmetric key algorithms rely on a “*shared secret*” encryption key that is distributed to all members who participate in the communications. This *key is used by all parties to both encrypt and decrypt* messages, so the sender and the receiver both possess a copy of the shared key.

When large-sized keys are used, symmetric encryption is very difficult to break. It is primarily employed to perform bulk encryption and provides only for the security service of [[Cryptography#Confidentiality|confidentiality]]

Symmetric key cryptography can also be called *secret key cryptography* and *private key* cryptography.

![[Symmetric Key Encryption.png]]

Symmetric key cryptography has several weaknesses:

- **Key distribution is a major problem**. Parties must have a secure method of exchanging the secret key before establishing communications with a symmetric key protocol. If a secure electronic channel is not available, an offline key distribution method must often be used (that is, out-of-band exchange).
  
- **Symmetric key cryptography does not implement nonrepudiation**. Because any communicating party can encrypt and decrypt messages with the shared secret key, there is no way to prove where a given message originated.
  
- **The algorithm is not scalable**. It is extremely difficult for large groups to communicate using symmetric key cryptography. Secure private communication between individuals in the group could be achieved only if each possible combination of users shared a private key.
  
- **Keys must be regenerated often**. Each time a participant leaves the group, all keys known by that participant must be discarded.

The major strength of symmetric key cryptography is the *great speed* at which it can operate. Symmetric key encryption is very fast, often **1,000 to 10,000 times faster** than asymmetric algorithms. By nature of the mathematics involved, symmetric key cryptography also naturally lends itself to hardware implementations, creating the opportunity for even higher-speed operations.

