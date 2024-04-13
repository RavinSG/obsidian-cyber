---
aliases:
  - AES
---
In October 2000, the National Institute of Standards and Technology announced that the Rijndael (pronounced “rhine-doll”) block cipher had been chosen as the replacement for DES.

The AES cipher allows the use of three key strengths: 128 bits, 192 bits, and 256 bits. AES only allows the processing of 128-bit blocks, but Rijndael exceeded this specification, allowing cryptographers to use a block size equal to the key length. The number of encryption rounds depends on the key length chosen:

- *128-bit* keys require 10 rounds of encryption
- *192-bit* keys require 12 rounds of encryption
- *256-bit* keys require 14 rounds of encryption

Today, AES is one of the most widely used encryption algorithms, and it plays an essential role in wireless network security, the Transport Layer Security ([[Transport Layer Security|TLS]]) Protocol, file/disk encryption, and many other applications that call for strong cryptography.