
Response controls are controls used to allow organisations to respond to an issue, whether it is an outage, a compromise, or a disaster. 

Recovery controls and techniques focus on returning to normal operations

An important response control in is the concept of **non-persistence**. This means the ability to have systems or services that are spun up and shut down as needed. Some systems are configured to revert to a known state when they are restarted; this is common in cloud environments where a code-defined system will be exactly the same as any other created and run with that code.

Another response control is the ability to **return to a last-known good configuration**. Windows systems build this in for the patching process, allowing a return to a checkpoint before a patch was installed. Change management processes often rely on a last-known good configuration checkpoint, via backups, snapshots, or another technology, to handle misconfigurations, bad patches, or other issues.

> [!info] Live Boot Media
> When a system has been compromised, or when the operating system has been so seriously impacted by an issue that it cannot properly function, one alternative is to use live boot media.
> 
> This is a bootable operating system that can run from removable media like a thumb drive or DVD. Using live boot media means that you can boot a full operating system that can see the hardware that a system runs on and that can typically mount and access drives and other devices.
> 
> This means that repair efforts can be run from a known good, trusted operating system. Boot sector and memory-resident viruses, bad OS patches and driver issues, and a variety of other issues can be addressed using this technique.

### [[Scalability]]

### [[Site Resilience]]
