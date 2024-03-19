Unlike Trojans that require user interaction, worms *spread themselves*. Although worms are often associated with spreading via attacks on vulnerable services, any type of spread through automated means is possible, meaning that worms can spread via email attachments, network file shares, or other methods as well. 

Worms also *self-install, rather than requiring users to click on them*, making them quite dangerous.

Common IoCs for worms include:

- Known malicious files
- Downloads of additional components from remote systems
- [[Command and Control]] contact to remote systems
- Malicious behaviours using system commands for injection and other activities, including use of `cmd.exe, msiexec.exe` and others
- Hands-on-keyboard attacker activity

Mitigating worm infections frequently starts with effective network-level controls focused on preventing infection traffic. *Firewalls, IPS devices, network segmentation*, and similar controls are the *first layer of defence*. Patching and configuring services to limit attack surfaces is also a best practice for preventing worms. 

After an infection responses may include use of anti-malware, [[Endpoint Detection and Response|EDR]], and similar tools to stop and potentially remove infections. Depending on the complexity of the malware, resetting to original firmware may be required for some devices.
