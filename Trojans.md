
Trojans, or Trojan horses, are a type of malware that is typically *disguised as legitimate software*. They are called Trojan horses because they rely on unsuspecting individuals running them, thus providing attackers with a path into a system or device.

**Remote access Trojans** (RATs) provide attackers with remote access to systems. Some legitimate remote access tools are used as RATs, which can make identifying whether a tool is a legitimate remote support tool or a tool being used for remote access by an attacker difficult.

IoCs for trojans often include:

- Signatures for the specific malware applications or downloadable files
- [[Command and Control]] system hostnames and IP addresses
- Folders or files created on target device

Mitigation practices for trojans typically starts with awareness practices that help ensure that downloading and running Trojans are less likely. Anti-malware, [[Endpoint Detection and Response|EDR]], and other tools used to identify and stop malicious software from running.