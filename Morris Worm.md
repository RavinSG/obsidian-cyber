---
tags:
  - Worm
---
The Morris [[worm]] or Internet worm of November 2, 1988, is one of the oldest computer worms distributed via the Internet.

It resulted in the first felony conviction in the US under the **1986 Computer Fraud and Abuse Act**.

The worm exploited several vulnerabilities of targeted systems, including:
- A hole in the debug mode of the Unix sendmail program
- A buffer overflow or overrun hole in the finger network service
- The transitive trust enabled by people setting up network logins with no password requirements via remote execution (rexec) with Remote Shell (rsh), termed rexec/rsh

It was initially programmed to check each computer to determine if the infection was already present, but Morris believed that some system administrators might counter this by instructing the computer to report a false positive. Instead, he programmed the worm to copy itself 14% of the time, regardless of the status of infection on the computer. This resulted in a computer potentially being infected multiple times, with each additional infection slowing the machine down to unusability. This had the same effect as a fork bomb, and crashed the computer several times.

It is usually reported that around 6,000 major UNIX machines were infected by the Morris worm. (10% of the internet)

The Internet was partitioned for several days, as regional networks disconnected from the NSFNet backbone and from each other to prevent recontamination while cleaning their own networks.

The Morris worm helped shape the security industry because it led to the development of computer emergency response teams, now commonly referred to as [[Computer Security Incident Response Teams]] (CSIRTs). 