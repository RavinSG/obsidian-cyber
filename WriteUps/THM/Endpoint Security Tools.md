
Once a system is running, ensuring that the system itself is secure is a complex task. Many types of security tools are available for endpoint systems, and the continuously evolving market for solutions means that traditional tool categories are often blurry.

### Antivirus and Antimalware

One of the most common security tools is antivirus and antimalware software. Although more *advanced anti-detection, obfuscation, and other defensive tools are always appearing* in new malware packages, using antimalware packages in enterprise environments remains a useful defensive layer in many situations.

Tools like these work to detect malicious software and applications through a variety of means. Here are the most common methods:

- **Signature-based detection**, which *uses a hash or other signature generation method* to identify files or components of the malware that have been previously observed. Attackers have increasingly used methods like polymorphism that change the malware every time it is installed, as well as encryption and packing to make signatures less useful.
  
- **Heuristic**, or behaviour-based detection, *looks at what actions the malicious software takes* and matches them to profiles of unwanted activities. Heuristic-based detection systems can identify new malware based on what it is doing, rather than just looking for a match to a known fingerprint.
  
- **AI and machine learning** systems are increasingly common throughout the security tools space. They *leverage large amounts of data* to find ways to identify malware that may include heuristic, signature, and other detection capabilities.
  
- **Sandboxing** is used by some tools and by the antimalware vendors themselves to *isolate and run sample malicious code*. Sandboxes are instrumented to allow all the actions taken by the software to be documented, providing the ability to perform in-depth analysis.

>[!warning] Malware Sandboxing
>Commercial and open source sandbox technologies are available, including [Cuckoo](cuckoosandbox.org) sandbox, an automated malware analysis tool. You'll also find sandboxing capabilities built into advanced antimalware tools, so you may already have sandboxing available to your organization.

As you consider deploying antimalware tools, it is important to keep a few key decisions in mind. First, you need to determine *what threats you are likely to face and where they are likely to be encountered*. In many organisations, a majority of malicious software threats are encountered on individual workstations and laptops, or are sent and received via email. Antimalware product deployments are thus focused on those two areas.

Second, *management, deployment, and monitoring for tools* is critical in an enterprise environment. Antimalware tools that allow central visibility and reporting integrates with other security tools for an easy view of the state of your systems and devices. 

Third, the *detection capabilities* you deploy and the overall likelihood *of your antimalware product* to detect, stop, and remove malicious software plays a major role in decision processes. Since malware is a constantly evolving threat, many organisations choose to *deploy more than one antimalware technology*, hoping to increase the likelihood of detection.

### Allow Lists and Deny Lists

One way to prevent malicious software from being used on systems is to *control the applications that can be installed or used*. That's where the use of allow list and deny or block list tools come in. 

**Allow list** tools allow you to build a list of software, applications, and other system components that are allowed to exist and run on a system. 

**Block lists**, or deny lists, are lists of software or applications that cannot be installed or run, rather than a list of what is allowed. 

The choice between the solutions depends on what administrators and security professionals want to accomplish. If a *system requires extremely high levels of security*, an *allow list* will provide greater security than a block or deny list, but if specific programs are considered undesirable, a *block list is less likely to interfere with unknown or new applications or programs*.

Although these tools may sound appealing, *they do not see widespread deployment* in most organisations because of the effort required to maintain the lists. Limited deployments and specific uses are more common throughout organisations, and other versions of allow and block lists are implemented in the form of firewalls and similar protective technologies.








