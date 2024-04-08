
Ciphers are the algorithms *used to perform encryption and decryption* operations. 

**Cipher suites** are the sets of *ciphers and key lengths supported by a system*. Modern ciphers fit into two major categories, describing their method of operation:

- **Block ciphers** operate on “chunks,” or blocks, of a message and apply the encryption algorithm to an *entire message block* at the same time. The transposition ciphers are examples of block ciphers. Complicated columnar transposition cipher works on an entire message (or a piece of a message) and encrypts it using the *transposition algorithm and a secret keyword*. Most modern encryption algorithms implement some type of block cipher.
  
- **Stream ciphers** operate on *one character or bit of a message* (or data stream) at a time. The Caesar cipher is an example of a stream cipher. The one-time pad is also a stream cipher because the algorithm operates on each letter of the plaintext message independently. *Stream ciphers can also function as a type of block cipher.* In such operations there is a buffer that fills up to real-time data that is then encrypted as a block and transmitted to the recipient.

