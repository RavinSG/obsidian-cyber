All digital information deserves to be kept *private, safe, and secure*. Encryption is one key to doing that! It is useful for transforming information into a form that unintended recipients cannot understand.

There are two main types of encryption:

- **[[Symmetric Key Encryption|Symmetric encryption]]** is the use of a *single secret key* to exchange information. Because it uses one key for encryption and decryption, the sender and receiver must know the secret key to lock or unlock the cipher.

- **[[Asymmetric Key Encryption|Asymmetric encryption]]** is the use of a *public and private key pair* for encryption and decryption of data. It uses two separate keys: a public key and a private key. The public key is used to encrypt data, and the private key decrypts it. The private key is only given to users with authorised access.

### Obscurity is not security

In the world of cryptography, a cipher must be proven to be unbreakable before claiming that it is secure. According to [[Kerchoff’s Principle]], cryptography should be designed in such a way that *all the details of an algorithm—except for the private key—should be knowable without sacrificing its security*. For example, you can access all the details about how AES encryption works online and yet it is still unbreakable.

Occasionally, organisations implement their own, custom encryption algorithms. There have been instances where those secret cryptographic systems have been quickly cracked after being made public.