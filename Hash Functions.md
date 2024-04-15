
Hash functions have a very simple purpose—they *take a potentially long message* and *generate a unique output* value derived from the content of the message.

There are five basic requirements for a cryptographic hash function:

- They accept an input of any length.
- They produce an output of a fixed length, regardless of the length of the input.
- The hash value is relatively easy to compute.
- The hash function is one-way (meaning that it is extremely hard to determine the input when provided with the output).
- The hash function is collision free (meaning that it is extremely hard to find two messages that produce the same hash value).

### Hashing Algorithms

- [[Secure Hashing Algorithms|SHA]]
- [[MD5]]