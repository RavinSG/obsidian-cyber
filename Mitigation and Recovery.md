An active incident can cause disruptions throughout an organization. The organization must act to mitigate the incident and then work to *recover from it without creating new risks or vulnerabilities*. At the same time, the organization may want to preserve incident data and artefacts to allow forensic analysis by internal responders or law enforcement.

### Security Orchestration, Automation, and Response ([[Security Orchestration, Automation, and Response|SOAR]])

*Managing multiple security technologies* can be challenging, and using the information from those platforms and systems to determine your organisation's security posture and status requires *integrating different data sources*. At the same time, *managing security operations and remediating issues* you identify is also an important part of security work. SOAR platforms seek to address these needs.

As a mitigation and recovery tool, SOAR platforms allow you to quickly assess the attack surface of an organization, the state of systems, and where issues may exist. They also allow automation of remediation and restoration workflows.

### Containment, Mitigation, and Recovery Techniques

In many cases, one of the first mitigation techniques will be to quickly block the cause of the incident on the impacted systems or devices. That means you may need to reconfigure endpoint security solutions:

- *Application allow lists* (sometimes referred to as whitelisting) list the applications and files that are allowed to be on a system and prevent anything that is not on the list from being installed or run.
  
- *Application deny lists* or block lists (sometimes referred to as blacklists) list applications or files that are not allowed on a system and will prevent them from being installed or copied to the system.
  
- *Isolation or quarantine* solutions can place files in a specific safe zone. Antimalware and antivirus often provide an option to quarantine suspect or infected files rather than deleting them, which can help with investigations.
  
- *Monitoring* is a key part of containment and mitigation efforts because security professionals and system administrators need to validate their efforts. Monitoring a system, service, or device can provide information about whether there are still issues or the device remains compromised. Monitoring can also show other actions taken by attackers after remediation is completed, helping responders identify the rest of the attacker's compromised resources.

>[!tip] Quarantine or Delete?
> One of the authors of this book dealt with a major issue caused by an antivirus update that incorrectly identified all Microsoft Office files as malware. That change resulted in thousands of machines taking their default action on those files. Fortunately, most of the organization used a quarantine, and then deleted settings for the antivirus product. One division, however, had set their systems to delete as the primary action. Every Office file on those systems was deleted within minutes of the update being deployed to them, causing chaos as staff tried to access their files. Although most of the files were eventually restored, some were lost as systems overwrote the deleted files with other information.
> 
> This isn't a typical scenario, but understanding the settings you are using and the situations where they may apply is critical. *Quarantine can be a great way to ensure that you still have access to the files, but it does run the danger of allowing the malicious files to still be on the system, even if they should be in a safe location*.

Configuration changes are also a common remediation and containment technique. They may be required to address a security vulnerability that allowed the incident to occur, or they may be needed to isolate a system or network. In fact, configuration changes are one of the most frequently used tools in containment and remediation efforts. They need to be carefully tracked and recorded, since responders can still make mistakes, and changes may have to be rolled back after the incident response process to allow a return to normal function. Common examples of remediation actions include:

Firewall rule changes, either to add new firewall rules, modify existing firewall rules, or in some cases, to remove firewall rules.
Mobile device management (MDM) changes, including applying new policies or changing policies; responding by remotely wiping devices; locating devices; or using other MDM capabilities to assist in the IR process.
Data loss prevention (DLP) tool changes, which may focus on preventing data from leaving the organization or detecting new types or classifications of data from being sent or shared. DLP changes are likely to be reactive in most IR processes, but DLP can be used to help ensure that an ongoing incident has a lower chance of creating more data exposure.