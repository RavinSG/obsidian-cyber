
The most famous public key cryptosystem is named after its creators. In 1977, *Ronald Rivest*, *Adi Shamir*, and *Leonard Adleman* proposed the RSA public key algorithm that remains a worldwide standard today.

The RSA algorithm depends on the **computational difficulty inherent in factoring large prime numbers**. Each user of the cryptosystem generates a pair of public and private keys using the algorithm

>[!important] Key Length
>The length of the cryptographic key is perhaps the most important security parameter that can be set at the discretion of the security administrator.
>
>It's important to understand the capabilities of your encryption algorithm and choose a key length that provides an appropriate level of protection. This judgment can be made by weighing the *difficulty of defeating a given key length* (measured in the amount of processing time required to defeat the cryptosystem) *against the importance of the data*
>
>Generally speaking, the more critical your data, the stronger the key you use to protect it should be. Timeliness of the data is also an important consideration. If it takes current computers one year of processing time to break your code, it will take only three months if the attempt is made with contemporary technology about four years down the road based on Moore's Law.
>
>The strengths of various key lengths also vary greatly according to the cryptosystem you're using. For example, a *1,024-bit RSA key* offers approximately the *same* degree of security as a *160-bit ECC key*.







