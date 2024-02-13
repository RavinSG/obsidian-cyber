In cryptography, a preimage attack on cryptographic hash functions tries to find a message that has a specific hash value. A cryptographic hash function should resist attacks on its preimage (set of possible inputs).

In the context of attack, there are two types of preimage resistance:

- **preimage resistance**: for essentially all pre-specified outputs, it is computationally infeasible to find any input that hashes to that output; i.e., given y, it is difficult to find an x such that $h(x) = y.$

- **second-preimage** resistance: for a specified input, it is computationally infeasible to find another input which produces the same output; i.e., given x, it is difficult to find a second input x′ ≠ x such that $h(x) = h(x′)$.