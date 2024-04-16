
### Lightweight Cryptography

Some devices operate at *extremely low power levels* and put a premium on conserving energy. For example, imagine sending a satellite into space with a limited power source. Similar cases happen here on Earth, where remote sensors must transmit information using solar power, a small battery, or other circumstances.

Smart cards are another example of a low power environment. They must be able to securely communicate with smart card readers, but only using the energy either stored on the card or transferred to it by a magnetic field.

In these cases, cryptographers often *design specialised hardware* that is purpose-built to implement lightweight cryptographic algorithms with as little power expenditure as possible.

Another specialised use case for cryptography are cases where you need *very low latency*. *Encrypting network links* is a common example of low latency cryptography. The data is moving quickly across a network and the encryption should be done as quickly as possible to avoid becoming a bottleneck.

Specialised encryption hardware also solves many low latency requirements. For example, a dedicated *VPN hardware* device may contain cryptographic hardware that implements encryption and decryption operations in highly efficient form to maximise speed.

### Homomorphic Encryption

We sometimes have applications where we want to *protect the privacy* of individuals, *but still want to perform calculations* on their data. Homomorphic encryption technology allows this, encrypting data in a way that preserves the ability to perform computation on that data.

When you encrypt data with a homomorphic algorithm and then perform computation on that data, you get a result that, when decrypted, matches the result you would have received if you had performed the computation on the plaintext data in the first place

### Quantum Computing

It's still mostly a theoretical field but, if it advances to the point where that theory becomes practical to implement, quantum cryptography may be able to *defeat cryptographic algorithms* that depend on factoring large prime numbers.

At the same time, quantum computing *may be used to develop even stronger cryptographic algorithms* that would be far more secure than modern approaches.