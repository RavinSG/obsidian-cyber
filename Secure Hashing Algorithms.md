---
aliases:
  - SHAs
  - SHA
---
**The Secure Hash Algorithm** (SHA) and its successors, SHA-1, SHA-2, and SHA-3, are government standard hash functions promoted by the National Institute of Standards and Technology (NIST) and are specified in an official government publication.

SHA-1 *takes an input of virtually any length* (in reality, there is an upper bound of approximately 2,097,152 terabytes on the algorithm) and produces a *160-bit message digest*. The SHA-1 algorithm processes a message in 512-bit blocks. Therefore, if the message length is not a multiple of 512, the SHA algorithm pads the message with additional data until the length reaches the next highest multiple of 512.

Cryptanalytic attacks demonstrated that there are *weaknesses in the SHA-1* algorithm. This led to the creation of SHA-2, which has four variants:

- **SHA-256** produces a 256-bit message digest using a 512-bit block size.
- **SHA-224** uses a truncated version of the SHA-256 hash to produce a 224-bit message digest using a 512-bit block size. 
- **SHA-512** produces a 512-bit message digest using a 1,024-bit block size. 
- **SHA-384** uses a truncated version of the SHA-512 hash to produce a 384-bit digest using a 1,024-bit block size.

The cryptographic community generally considers the SHA-2 algorithms secure, but they theoretically suffer from the same weakness as the SHA-1 algorithm.

The *SHA-3* suite was developed to serve as drop-in replacement for the SHA-2 hash functions, offering the same variants and hash lengths using a more secure algorithm.

To **avoid the risk of hash collisions**, functions that generated longer values were needed. MD5's shortcomings gave way to a new group of functions known as the Secure Hashing Algorithms, or SHAs.

The National Institute of Standards and Technology ([[National Institute of Standards and Technology|NIST]]) approves each of these algorithms. Numbers besides each SHA function indicate the size of its hash value in bits. Except for SHA-1, which produces a 160-bit digest, these algorithms are considered to be collision-resistant. However, that doesn’t make them invulnerable to other exploits.

There are 5 SHA Algorithms
- SHA-1
- SHA-224
- SHA-256
- SHA-384
- SHA-512

