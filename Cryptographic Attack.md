A cryptographic attack affects secure forms of communication between a sender and intended recipient. Some forms of cryptographic attacks are: 

- [[Birthday Attack|Birthday]]
- Collision
- Downgrade

Cryptographic attacks fall under the [[Communications and Network Security]] domain. 

#### Brute Force

This method simply involves trying every possible key. It is guaranteed to work, but it is likely to take so long that it is simply not usable. For example, to break a Caesar cipher, there are only 26 possible keys, which you can try in a very short time. But even DES, which has a rather weak key, would take $2^{56}$ different attempts. That is 72,057,594,037,927,936 possible DES keys. To put that in perspective, if you try 1 million keys per second, it would take you just a bit over 46,190,765 years to try them all.

#### Frequency Analysis

Frequency analysis involves looking at the blocks of an encrypted message to determine *if any common patterns exist*. Initially, the analyst doesn't try to break the code but looks at the patterns in the message. In the English language, the letters e and t and words like the, and, that, it, and is are very common. Single letters that stand alone in a sentence are usually limited to a and I.