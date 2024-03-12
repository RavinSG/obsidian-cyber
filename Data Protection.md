As we think through data protection techniques, it's helpful to consider the three states where data might exist:

- **Data at rest** is stored data that resides on hard drives, tapes, in the cloud, or on other storage media. This data is prone to pilfering by insiders or external attackers who gain access to systems and are able to browse through their contents.

- **Data in transit** is data that is in motion/transit over a network. When data travels on an untrusted network, it is open to eavesdropping attacks by anyone with access to those networks.

- **Data in use** is data that is actively in use by a computer system. This includes the data stored in memory while processing takes place. An attacker with control of the system may be able to read the contents of memory and steal sensitive information.

We can use *different security controls* to safeguard data in all of these states, building a robust set of defences that protects our organisation's vital interests.

### Data Encryption

[[Encryption]] technology uses mathematical algorithms to protect information from prying eyes, both while it is in transit over a network and while it resides on systems. 

Encrypted data is unintelligible to anyone who does not have access to the appropriate decryption key, *making it safe to store and transmit encrypted data over otherwise insecure means*.

### [[Data Loss Prevention]]

### [[Data Minimisation]]

### [[Access Restriction]]

### Segmentation and Isolation

Organisations may also limit the access to sensitive information based on their  network location. 

*Segmentation* places sensitive systems on seperate networks where they may communicate with each other but have strict restrictions on their ability to communicate with systems on other networks.

*Isolation* goes a step further and completely cuts a system off from access to or from outside the networks.