---
aliases:
  - ECC
---
In 1985, two mathematicians, *Neal Koblitz* from the University of Washington and *Victor Miller* from IBM, independently proposed the application of elliptic curve cryptography (ECC) theory to develop secure cryptographic systems.

Any elliptic curve can be defined by the following equation:

$$y^2 = x^3 + ax + b$$

In this equation, x, y, a, and b are all real numbers. Each elliptic curve has a corresponding elliptic curve group made up of the points on the elliptic curve along with the point O, located at infinity.

Two points within the same elliptic curve group ($P$ and $Q$) can be added together with an elliptic curve addition algorithm. This operation is expressed as $P + Q$.

This problem can be extended to involve multiplication by assuming that $Q$ is a multiple of $P$, meaning the following: $Q = xP$.

Computer scientists and mathematicians believe that it is extremely hard to find $x$, even if $P$ and $Q$ are already known. 

This difficult problem, known as the *elliptic curve discrete logarithm problem*, forms the basis of elliptic curve cryptography. 

It is widely believed that **this problem is harder** to solve than both the **prime factorisation** problem that the RSA cryptosystem is based on and the **standard discrete logarithm** problem utilised by Diffie–Hellman.