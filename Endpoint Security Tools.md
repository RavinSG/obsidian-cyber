
Once a system is running, ensuring that the system itself is secure is a complex task. Many types of security tools are available for endpoint systems, and the continuously evolving market for solutions means that traditional tool categories are often blurry.

### Antivirus and Antimalware

One of the most common security tools is antivirus and antimalware software. Although more *advanced anti-detection, obfuscation, and other defensive tools are always appearing* in new malware packages, using antimalware packages in enterprise environments remains a useful defensive layer in many situations.

Tools like these work to detect malicious software and applications through a variety of means. Here are the most common methods:

- **Signature-based detection**, which *uses a hash or other signature generation method* to identify files or components of the malware that have been previously observed. Attackers have increasingly used methods like polymorphism that change the malware every time it is installed, as well as encryption and packing to make signatures less useful.
  
- **Heuristic**, or behaviour-based detection, *looks at what actions the malicious software takes* and matches them to profiles of unwanted activities. Heuristic-based detection systems can identify new malware based on what it is doing, rather than just looking for a match to a known fingerprint.
  
- **AI and machine learning** systems are increasingly common throughout the security tools space. They *leverage large amounts of data* to find ways to identify malware that may include heuristic, signature, and other detection capabilities.
  
- **Sandboxing** is used by some tools and by the antimalware vendors themselves to *isolate and run sample malicious code*. Sandboxes are instrumented to allow all the actions taken by the software to be documented, providing the ability to perform in-depth analysis.

>[!warning] Malware Sandboxing
>Commercial and open source sandbox technologies are available, including Cuckoo sandbox, an automated malware analysis tool. You'll also find sandboxing capabilities built into advanced antimalware tools, so you may already have sandboxing available to your organization.

As you consider deploying antimalware tools, it is important to keep a few key decisions in mind. First, you need to determine *what threats you are likely to face and where they are likely to be encountered*. In many organisations, a majority of malicious software threats are encountered on individual workstations and laptops, or are sent and received via email. Antimalware product deployments are thus focused on those two areas.

Second, *management, deployment, and monitoring for tools* is critical in an enterprise environment. Antimalware tools that allow central visibility and reporting integrates with other security tools for an easy view of the state of your systems and devices. 

Third, the *detection capabilities* you deploy and the overall likelihood *of your antimalware product* to detect, stop, and remove malicious software plays a major role in decision processes. Since malware is a constantly evolving threat, many organisations choose to *deploy more than one antimalware technology*, hoping to increase the likelihood of detection.

### Allow Lists and Deny Lists

One way to prevent malicious software from being used on systems is to *control the applications that can be installed or used*. That's where the use of allow list and deny or block list tools come in. 

**Allow list** tools allow you to build a list of software, applications, and other system components that are allowed to exist and run on a system. 

**Block lists**, or deny lists, are lists of software or applications that cannot be installed or run, rather than a list of what is allowed. 

The choice between the solutions depends on what administrators and security professionals want to accomplish. If a *system requires extremely high levels of security*, an *allow list* will provide greater security than a block or deny list, but if specific programs are considered undesirable, a *block list is less likely to interfere with unknown or new applications or programs*.

Although these tools may sound appealing, *they do not see widespread deployment* in most organisations because of the effort required to maintain the lists. Limited deployments and specific uses are more common throughout organisations, and other versions of allow and block lists are implemented in the form of firewalls and similar protective technologies.

### Endpoint Detection and Response and Extended Detection and Response

When antimalware tools are not sufficient, endpoint detection and response ([[Endpoint Detection and Response|EDR]]) tools can be deployed. Key features of EDR systems are the ability to search and explore the collected data and to use it for investigations as well as the ability to detect suspicious data.

With the continued growth of security analytics tools, EDR systems tend to look for anomalies and indicators of compromise ([[Indicators of Compromise|IoCs]]) using automated rules and detection engines as well as allowing manual investigation.

If you are considering an EDR deployment, you will want to pay attention to organisational needs like the *ability to respond to incidents*; the ability to *handle threats from a variety of sources*, ranging from malware to data breaches; and the ability to *filter and review large quantities of endpoint data* in the broader context of your organisations.

In addition to EDR, **extended detection and response** ([[Extended Detection and Response|XDR]]) tools are increasingly commonly deployed by organisations. XDR is similar to EDR but has a *broader perspective* considering not only endpoints but the full breadth of an organisation's technology stack, including **cloud services**, **security services and platforms**, **email**, and similar components. 

They ingest logs and other information from the broad range of components, then use detection algorithms as well as AI and ML to analyse the data to find issues and help security staff respond to them.

### Data Loss Prevention

Protecting organisational data from **both theft and inadvertent exposure** drives the use of data loss prevention ([[Data Loss Prevention|DLP]]) tools. DLP tools may be *deployed to endpoints* in the form of clients or applications. These tools also commonly have *network and server-resident components* to ensure that data is managed throughout its lifecycle and various handling processes.

Key elements of DLP systems include the ability to *classify data* so that organisations know which data should be protected; *data labelling or tagging* functions, to support classification and management practices; *policy management and enforcement functions* used to manage data to the standards set by the organization; and *monitoring and reporting* capabilities, to quickly notify administrators or security practitioners about issues or potential problems.

Some DLP systems also provide additional functions that **encrypt** data when it is sent outside of protected zones. In addition, they may include capabilities intended to allow sharing of data *without creating potential exposure*, either **tokenising**, **wiping**, or otherwise **modifying** data to permit its use without violation of the data policies set in the DLP environment.

Much like antimalware and EDR systems, some DLP systems also *track user behaviours* to *identify questionable behaviour* or *common mistakes* like assigning overly broad permissions on a file or sharing a sensitive document via email or a cloud file service.

### Network Defences

Protecting endpoints from network attacks can be done with a **host-based firewall** that can stop unwanted traffic. Host-based firewalls are built into most modern operating systems and are typically enabled by default. 

Of course, host-based firewalls *don't provide much insight* into the traffic they are filtering since they often *simply block or allow specific applications*, *services*, *ports*, or *protocols*. More advanced filtering requires greater insight into what the traffic being analysed is, and that's where a host intrusion prevention or intrusion detection system comes in.

A **host intrusion prevention system** ([[Host Intrusion Prevention System|HIPS]]) *analyses traffic before services or applications on the host process* it. A HIPS can take action on that traffic, including filtering out malicious traffic or blocking specific elements of the data that is received. A HIPS will look at traffic that is *split across multiple packets* or throughout an entire series of communications, allowing it to catch malicious activity that may be spread out or complex.

Since a HIPS can actively block traffic, misidentification of traffic as malicious, misconfiguration, or other issues can cause legitimate traffic to be blocked, potentially causing an outage.

A host intrusion detection system (**HIDS**) performs similar functions but, like a traditional intrusion detection system (**IDS**) *it cannot take action to block traffic*. Instead, a HIDS can only report and alert on issues. Therefore, a HIDS has limited use for real-time security, but it has a much lower likelihood of causing issues.

Before deploying and managing host-based firewalls, HIPSs, or HIDSs, determine how you will manage them, how complex the individual configurations will be, and *what would happen if the host-based protections had problems*. 

Granular controls are an important part of a zero-trust design, and armouring hosts helps ensure that a compromised system behind a security boundary does not result in broader issues for organisations.

The image below shows the typical placement of a host-based firewall, a HIPS, and a HIDS, as well as where a network firewall or IPS/IDS device might be placed. 

![[Host vs Network IPS and IDS.png]]