
Forensic data is acquired using forensic tools like *disk and memory imagers, image analysis and timelining tools, low-level editors* that can display detailed information about the contents and structure of data on a disk, and other specialised tools.

### Acquiring Forensic Data

When a forensic practitioner plans to acquire data, one of the first things they will review is the **order of volatility**. The order of volatility documents what data is most likely to be lost due to system operations or normal processes. 

![[Order of Volatility.png]]

Above image shows a typical order of volatility chart. Note that frequently changing information like the state of the CPU's registers and cache is first and thus most volatile, and that information about routes, processes, and kernel statistics follows. 

As the list proceeds, each item is less likely to disappear quickly, with backups being the least likely to change. Following the order of volatility for acquisitions—unless there is a compelling and immediate reason to differ from the list—will provide a forensic analyst with the *greatest likelihood of capturing data intact*. 

It is important to remember which items will disappear when a system is powered down or rebooted. In general, that occurs at position 4 for temporary files and swap space on this list. Recovering intact temporary files and data from swap space will depend on how the system was shut down and if it was rebooted successfully afterward. 
 
Common forensic locations include the following:

- **CPU cache and registers** are rarely directly captured as part of a normal forensic effort. Although it is possible to capture some of this information using specialized hardware or software, most investigations do not need this level of detail. The CPU cache and registers are constantly changing as processing occurs, making them very volatile.
  
- **Ephemeral data** such as the process table, kernel statistics, the system's ARP cache, and similar information can be captured through a combination of memory and disk acquisition, but it is important to remember that the capture will only be of the moment in time when the acquisition is done. If events occurred in the past, this data may not reflect the state that the system was in when the event occurred.
  
- The content of **random access memory** (RAM) can be very helpful for both investigations and incident response. Memory *can contain encryption keys, ephemeral data from applications, and information that may not be written to the disk* but that can be useful to an investigation.

- **Swap and pagefile** information is disk space used to supplement physical memory. Much like capturing information from RAM, capturing the swap and pagefile can provide insight into running processes. Since it is actively used by the system, particularly on machines with less memory, it also changes more quickly than many files on disk.
  
- **Files and data on a disk** change more slowly but are the *primary focus of many investigations*. It is important to capture the entire disk rather than just copy files so that you can see deleted files and other artefacts that remain resident.
  
- The **operating system** itself can contain useful information. The *Windows Registry* is a common target for analysis since many activities in Windows modify or update the Registry.
  
- Devices such as **smartphones, tablets, IoT devices, and embedded** or specialised systems may contain data that can also be forensic targets.
  
- **Firmware** is a less frequently targeted forensic artefact, but knowing how to copy the firmware from a device can be necessary *if the firmware was modified as part of an incident* or if the firmware may have forensically relevant data. Firmware is often accessible using a hardware interface like a serial cable or direct USB connection or via memory forensic techniques.
  
- **Snapshots from virtual machines** (VMs) are an increasingly common artefact that forensic practitioners must deal with.
  
- **Network traffic and logs** can provide detailed information or clues about what was sent or received, when, and via what port and protocol, among other useful details.
  
- **Artefacts like devices, printouts, media**, and other items related to investigations can all provide additional useful forensic data.

### Chain-of-Custody

Regardless of the type of forensic data that is obtained or handled, it is *important to maintain chain-of-custody documentation if the forensic case may result in a legal case*. In fact, some organisations apply these rules regardless of the case to ensure that a case can be supported if it becomes necessary. Chain-of-custody forms are simple sign-off and documentation forms, as shown below. Each time the drive, device, or artefact is accessed, transferred, or otherwise handled, it is documented as shown on the form.

![[Chain-of-Custody Form.png]]

Evidence in court cases is typically legally admissible if it is offered to prove the facts of a case, and it does not violate the law. To determine if evidence is admissible, criteria such as the **relevance and reliability of the evidence**, whether the evidence was **obtained legally, and whether the evidence is authentic** are applied. Evidence must be the best evidence available, and the process and procedures should stand up to challenges in court.

In addition to these requirements, admissibility for digital forensics requires that the *data be intact and unaltered and have provably remained unaltered before and during the forensic process*. Forensic analysts must be able to demonstrate that they have appropriate skills, that they used appropriate tools and techniques, and that they have documented their actions in a reliable and testable way via an auditable trail. Thus, their efforts and findings *must be repeatable by a third party if necessary*.

### [[Cloud Forensics]]

