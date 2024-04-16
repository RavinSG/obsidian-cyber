A cryptographic attack affects secure forms of communication between a sender and intended recipient. Some forms of cryptographic attacks are: 

- [[Birthday Attack|Birthday]]
- Collision
- Downgrade

Cryptographic attacks fall under the [[Communications and Network Security]] domain. 

#### Brute Force

This method simply involves trying every possible key. It is guaranteed to work, but it is likely to take so long that it is simply not usable. For example, to break a Caesar cipher, there are only 26 possible keys, which you can try in a very short time. But even DES, which has a rather weak key, would take $2^{56}$ different attempts. That is 72,057,594,037,927,936 possible DES keys. To put that in perspective, if you try 1 million keys per second, it would take you just a bit over 46,190,765 years to try them all.

#### Frequency Analysis

Frequency analysis involves looking at the blocks of an encrypted message to determine *if any common patterns exist*. Initially, the analyst doesn't try to break the code but looks at the patterns in the message. In the English language, the letters e and t and words like the, and, that, it, and is are very common. Single letters that stand alone in a sentence are usually limited to a and I.

#### Known Plain Text

This attack relies on the attacker having *pairs of known plain text* along with the corresponding *ciphertext*. This gives the attacker a place to start attempting to derive the key. With modern ciphers, it would still take many billions of such combinations to have a chance at cracking the cipher. This method was, however, successful at *cracking the German Naval Enigma*

#### Chosen Plain Text

In this attack, the attacker obtains the ciphertexts *corresponding to a set of plain texts* of their own choosing. This allows the attacker to attempt to derive the key used and thus decrypt other messages encrypted with that key. This can be difficult, but it is not impossible. Advanced methods such as *differential cryptanalysis* are types of chosen plain-text attacks

#### Related Key Attack

This is like a chosen plain-text attack, except the attacker can obtain cipher texts encrypted under two different keys. This is actually a useful attack if you can obtain the plain text and matching ciphertext.

#### Birthday Attack

Look at [[Birthday Attack]]. For an MD5 hash, you might think that you need $2^{128} +1$ different inputs to get a collision—and for a guaranteed collision you do. That is an exceedingly large number: $3.4028236692093846346337460743177e+38$

The math works out to about $1.7 \sqrt n$ to get a collision. That number is still very large:
$31,359,464,925,306,237,747.2$. But it is much smaller than the bruteforce approach of trying every possible input.

#### Downgrade Attack

A downgrade attack is sometimes used against secure communications such as TLS in an attempt to *get the user or system to inadvertently shift to less secure cryptographic modes*. The idea is to trick the user into shifting to a less secure version of the protocol, one that might be easier to break.

#### Hashing, Salting, and Key Stretching

[[Rainbow Table]] attacks attempt to reverse hashed password value by *precomputing* the *hashes of common passwords*. The attacker takes a list of common passwords and runs them through the hash function to generate the rainbow table. They then search through lists of hashed values, looking for matches to the rainbow table.

The most common approach to preventing these attacks is **salting**, which adds a randomly generated value to each password prior to hashing.

Key stretching is used to *create encryption keys from passwords in a strong manner*. Key stretching algorithms, such as the Password Based Key Derivation Function v2 (**PBKDF2**), use thousands of iterations of salting and hashing to generate encryption keys that are resilient against attack.

#### Exploiting Weak Keys

There are also scenarios in which someone is using a good cryptographic algorithm (like AES) but has it implemented in a weak manner—for example, *using weak key generation*. A classic example is the Wireless Equivalent Privacy ([[Wired Equivalent Privacy|WEP]]) protocol. This protocol uses an improper implementation of the RC4 encryption algorithm and has significant security vulnerabilities.

#### Exploiting Human Error

Human error is one of the major causes of encryption vulnerabilities. If an email is sent using an encryption scheme, someone else may send it in the clear (unencrypted). If a cryptanalyst gets ahold of both messages, the process of decoding future messages will be considerably simplified.

A code key might wind up in the wrong hands, giving insights into what the key consists of. Many systems have been broken into as a result of these types of accidents.

Another error is to use weak or deprecated algorithms. Over time, some algorithms are no longer considered appropriate. This may be due to some flaw found in the algorithm. It can also be due to increasing computing power. Example: [[Data Encryption Standard|DES]]

