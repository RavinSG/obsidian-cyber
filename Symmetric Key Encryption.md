
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

### Common Symmetric Cryptosystems

- [[Data Encryption Standard]]
- [[Advanced Encryption Standard]]

### Key Management

Users and administrators to take extraordinary measures to protect the security of the keying material. These security measures are collectively known as key management practices. 

They include safeguards surrounding the *creation*, *distribution*, *storage*, *destruction*, *recovery*, and *escrow* of secret keys.

#### Creation and Distribution

Key exchange is one of the *major problems underlying symmetric encryption* algorithms. Key exchange is the secure distribution of the secret keys required to operate the algorithms. The three main methods used to exchange secret keys securely are:

- **Offline Distribution**: One party provides the other party with a sheet of paper or piece of storage media containing the secret key. However, every offline key distribution method has its own inherent flaws. If keying material is sent through the mail, it might be intercepted. Telephones can be wiretapped. Papers containing keys might be inadvertently thrown in the trash or lost.
  
- **Public Key Encryption**: Many people use public key encryption to set up an initial communications link. Once the link is successfully established and the parties are satisfied as to each other's identity, they exchange a secret key over the secure public key link. They then *switch communications from the public key algorithm to the secret key* algorithm and enjoy the *increased processing speed*.
  
- **Diffie–Hellman**: In some cases, neither public key encryption nor offline distribution is sufficient. Two parties might need to communicate with each other, but they have no physical means to exchange key material, and there is no public key infrastructure in place to facilitate the exchange of secret keys. In situations like this, key exchange algorithms like the Diffie–Hellman algorithm prove to be extremely useful mechanisms.

>[!info] Diffie-Hellman Algorithm
>- The communicating parties (we'll call them Alice and Bob) agree on two large numbers: $p$ (which is a **prime** number) and $g$ (which is an **integer**) such that $1 < g < p$.
>- Alice chooses a random large integer $a$ and performs the following calculation:
>  $$A = g^a \text{ mod } p$$
>- Bob chooses a random large integer $b$ and performs the following calculation:
>  $$B = g^b \text{ mod } p$$
>- Alice sends $A$ to Bob and Bob sends $B$ to Alice.
>- Alice then performs the following calculation:
>  $$K = B^a \text{ mod } p$$
>- Bob then performs the following calculation:
>  $$K = A^b \text{ mod } p$$
>  
>At this point, Alice and Bob both have the same value, K, and can use this for secret key communication between the two parties.

#### Storage and Destruction

Another major challenge with the use of symmetric key cryptography is that all of the keys used in the cryptosystem must be kept secure. This includes following best practices surrounding the storage of encryption keys:

- *Never store an encryption key on the same system where encrypted data resides*. This just makes it easier for the attacker!
- For sensitive keys, consider providing *two different individuals with half of the key*. They then must collaborate to re-create the entire key. This is known as the **principle of split knowledge**.

When a *user with knowledge of a secret key leaves* the organization *or is no longer permitted access* to material protected with that key, the keys must be changed, and **all encrypted materials must be re-encrypted** with the new keys. 

The difficulty of destroying a key to remove a user from a symmetric cryptosystem is one of the main reasons organisations turn to asymmetric algorithms.

#### Key Escrow and Recovery

While cryptography offers tremendous security benefits, it can also be a little risky. If someone uses strong cryptography to protect data and then loses the decryption key, they won't be able to access their data again. 

**Key escrow** systems addresses this situation by having a third party store a protected copy of the key for use in an emergency. Organisation may have a formal key recovery policy that specifies the circumstances under which a key may be retrieved from escrow and used without a user's knowledge.

