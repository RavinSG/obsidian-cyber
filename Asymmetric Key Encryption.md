
Asymmetric key algorithms, also known as *public key algorithms*, provide a solution to the weaknesses of symmetric key encryption. In these systems, each user has two keys: a **public key**, which is *shared with all users*, and a **private key**, which is *kept secret* and known only to the owner of the key-pair. 

Opposite and related keys must be used in tandem to encrypt and decrypt. In other words, if the public key encrypts a message, then only the corresponding private key can decrypt it, and vice versa.

![[Asymmetric Key Encryption.png]]

Asymmetric key algorithms also provide *support for digital signature technology*. Basically, if Bob wants to assure other users that a message with his name on it was actually sent by him, he first creates a message digest by using a hashing algorithm. Bob then **encrypts that digest using his private key.** Any user who wants to verify the signature simply decrypts the message digest using Bob's public key and then verifies that the decrypted message digest is accurate.

The following is a list of the major strengths of asymmetric key cryptography:

- **The addition of new users requires the generation of only one public-private key pair**. This same key pair is used to communicate with all users of the asymmetric cryptosystem. This makes the algorithm extremely scalable.
  
- **Users can be removed far more easily from asymmetric systems**. Asymmetric cryptosystems provide a key revocation mechanism that allows a key to be canceled, effectively removing a user from the system.
  
- **Key regeneration is required only when a user's private key is compromised**. If a user leaves the community, the system administrator simply needs to invalidate that user's keys. No other keys are compromised and therefore key regeneration is not required for any other user.
  
- **Asymmetric key encryption can provide integrity, authentication, and nonrepudiation**. If a user does not share their private key with other individuals, a message signed by that user can be shown to be accurate and from a specific source and cannot be later repudiated.
  
- **Key exchange is a simple process**. Users who want to participate in the system simply make their public key available to anyone with whom they want to communicate. There is no method by which the private key can be derived from the public key.
  
- **No preexisting communication link needs to exist**. Two individuals can begin communicating securely from the start of their communication session. Asymmetric cryptography does not require a preexisting relationship to provide a secure mechanism for data exchange.

Asymmetric cryptography entails a higher degree of computational complexity than symmetric cryptography. *Keys used within asymmetric encryption systems must be longer* that those used in symmetric systems to produce cryptosystems of equivalent strengths.

### Asymmetric Cryptosystems

- [[RSA]]
- [[Elliptic Curve]]