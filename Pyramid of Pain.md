
Not all indicators of compromise are equal in the value they provide to security teams. It’s important for security professionals to understand the different types of indicators of compromise so that they can quickly and effectively detect and respond to them. This is why security researcher David J. Bianco created the concept of the Pyramid of Pain, with the goal of *improving how indicators of compromise are used in incident detection*.

![[Pyramid of Pain.png]]

The Pyramid of Pain captures the *relationship between indicators of compromise* and the *level of difficulty that malicious actors experience when indicators of compromise are blocked* by security teams. It lists the different types of indicators of compromise that security professionals use to identify malicious activity. 

Each type of indicator of compromise is separated into levels of difficulty. These levels represent the “pain” levels that an attacker faces when security teams block the activity associated with the indicator of compromise. 

For example, blocking an IP address associated with a malicious actor is labeled as easy because malicious actors can easily use different IP addresses to work around this and continue with their malicious efforts. If security teams are able to block the IoCs located at the top of the pyramid, the more difficult it becomes for attackers to continue their attacks. Here’s a breakdown of the different types of indicators of compromise found in the Pyramid of Pain. 

- *Hash values*: Hashes that correspond to known malicious files. These are often used to provide unique references to specific samples of malware or to files involved in an intrusion.

- *IP addresses*: An internet protocol address like 192.168.1.1

- *Domain names*: A web address such as www.google.com 

- *Network artefacts*: Observable evidence created by malicious actors on a network. For example, information found in network protocols such as User-Agent strings. 

- *Host artefacts*: Observable evidence created by malicious actors on a host. A host is any device that’s connected on a network. For example, the name of a file created by malware.

- *Tools*: Software that’s used by a malicious actor to achieve their goal. For example, attackers can use password cracking tools like John the Ripper to perform password attacks to gain access into an account.

- *Tactics, techniques, and procedures (TTPs)*: This is the behaviour of a malicious actor. Tactics refer to the high-level overview of the behaviour. Techniques provide detailed descriptions of the behaviour relating to the tactic. Procedures are highly detailed descriptions of the technique. TTPs are the hardest to detect. 

