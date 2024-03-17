
Ransomware is malware that takes over a computer and then demands a ransom. There are many types of ransomware, including crypto malware, which *encrypts files and then holds them hostage* until a ransom is paid.

Other ransomware techniques include *threatening to report* the user to law enforcement due to *pirated software or pornography*, or *threatening to expose sensitive information* or pictures from the victim's hard drive or device.

One of the most important defences against ransomware is an *effective backup system* that stores files in a separate location that will not be impacted if the system or device it backs up is infected and encrypted by ransomware.

IoCs for ransomware include, but not limited to:

- Command and control (C&C) traffic and/or other contact to known malicious IP addresses
- Use of legitimate tools in abnormal ways to retain control of the compromised system
- Lateral movement processes that seek to attack or gain information about other systems or devices inside the same trust boundary
- Encryption of files
- Notices to end users of the encryption process with demands for ransom
- Data exfiltration behaviours, including large file transfers