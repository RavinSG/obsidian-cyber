
Threat hunting helps organisations achieve the detection and analysis phases of the incident response process. Threat hunters look for indicators of compromise ([[Indicators of Compromise|IoCs]]) that are commonly associated with malicious actors and incidents such as: 

- **Account lockout**, which is often due to brute-force login attempts or incorrect passwords used by attackers.
  
- **Concurrent session usage** when users aren't likely to use concurrent sessions. If a user is connected from more than one system or device, particularly when the second device is in an unexpected or uncommon location or the application is one that isn't typically used on multiple devices at once, this can be a strong indicator that something is not right.
  
- **Blocked content** is content that the organization has blocked, often via a DNS filter or other tool that prohibits domains, IP addresses, or types of content from being viewed or accessed. If this occurs, it may be because a malicious actor or malware is attempting to access the resource.
  
- **Impossible travel**, which involves a user connecting from two locations that are far enough apart that the time between the connections makes the travel impossible to have occurred, typically indicates that someone else has access to the user's credentials or devices.
  
- **Resource consumption** like filling up a disk or using more bandwidth than usual for uploads or downloads, can be an indicator of compromise. Unlike some of the other IoCs here, this one often requires other actions to become concerning unless it is much higher than usual.
  
- **Resource inaccessibility** can indicate that something unexpected is happening. If a resource like a system, file, or service isn't available identifying the underlying cause and ensuring that the cause isn't malicious, can be important.
  
- **Out-of-cycle logging** occurs when an event that happens at the same time or on a set cycle occurs at an unusual time. This might be a worker logging in at 2 a.m. who normally works 9–5, or a cleanup process that gets activated when it normally runs once a week.
  
- **Missing logs** may indicate that an attacker has wiped the logs to attempt to hide their actions. This is one reason that many organisations centralise their log collection so that a protected system will retain logs even if they are wiped on a server or workstation.


The discipline of threat hunting is closely related to [[Penetration Testing]] but has a separate and distinct purpose. Threats evolve and attackers advance their tactics and techniques. Automated, technology-driven detection can be limited in *keeping up to date with the evolving threat landscape*. Human-driven detection like threat hunting combines the power of technology with a human element to discover hidden threats left undetected by detection tools.

Threat hunting is the *proactive search for threats on a network*. Security professionals use threat hunting to uncover malicious activity that was not identified by detection tools and as a way to do further analysis on detections. Threat hunting is also used to detect threats before they cause damage. For example, *fileless malware* is difficult for detection tools to identify. It’s a form of malware that uses sophisticated evasion techniques such as hiding in memory instead of using files or applications, allowing it to *bypass traditional methods of detection like signature analysis*. With threat hunting, the combination of active human analysis and technology is used to identify threats like fileless malware. 

Threat hunting builds on a cybersecurity philosophy known as the “**presumption of compromise.**” This approach assumes that attackers have already successfully breached an organization and searches out the evidence of successful attacks. When threat hunters discover a potential compromise, they then kick into incident handling mode, seeking to contain, eradicate, and recover from the compromise.

Threat hunters work with a variety of intelligence sources, using the concept of intelligence fusion to combine information from *threat feeds security advisories and bulletins*, and other sources. They then seek to trace the path that an attacker followed as they manoeuvre through a target network