
The use of redundant arrays of inexpensive disks (**RAID**) is a common solution that uses multiple disks with data either *striped* (spread across disks) or *mirrored* (completely copied), and technology to ensure that data is not corrupted or lost (parity).

The table below shows the most common RAID solutions with their advantages and disadvantages.

| RAID description                                   | Description                                                                                                                                             | Advantage                                                                                                                                        | Disadvantage                                                                                                                                           |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| RAID 0 –<br>Striping                               | Data is spread across<br>all drives in the array.                                                                                                       | Better I/O<br>performance<br>(speed), all<br>capacity used.                                                                                      | Not fault<br>tolerant—all<br>data lost if a<br>drive is lost.                                                                                          |
| RAID 1 –<br>Mirroring                              | All data is copied<br>exactly to another<br>drive or drives.                                                                                            | High read<br>speeds from<br>multiple<br>drives, data<br>available if a<br>drive fails.                                                           | Uses twice the<br>storage for the<br>same amount of<br>data.                                                                                           |
| RAID 5 –<br>Striping with<br>parity                | Data is striped across<br>drives, with one drive<br>used for parity<br>(checksum) of the<br>data. Parity is spread<br>across drives as well<br>as data. | Data reads<br>are fast, data<br>writes are<br>slightly<br>slower. <br>Drive failures<br>can be rebuilt<br>as long as<br>only one<br>drive fails. | Can only<br>tolerate a single<br>drive failure at a<br>time.<br>Rebuilding<br>arrays after a<br>drive loss can<br>be slow and<br>impact<br>performance |
| RAID 10 –<br>Mirroring<br>and striping<br><br><br> | Data is striped across<br>two or more drives<br>and then mirrored to<br>the same number of<br>drives.                                                   | Combines the<br>advantages<br>and<br>disadvantages<br>of both RAID<br>0 and RAID 1.                                                              | Combines the<br>advantages and<br>disadvantages<br>of both RAID 0<br>and RAID 1.<br>Sometimes<br>written as RAID<br>1+0.                               |

### Backup

In addition to disk-level protections, backups and replication are frequently used to ensure that data loss does not impact an organization. 

Backups are a copy of the live storage system: 
- **Full** backup, which copies the entire device or storage system
- **Incremental** backup, which captures the changes since the last backup and is faster to back up but slower to recover
- **Differential** backup, which captures the changes since the last full backup and is faster to recover but slower to back up

Since most failures are not a complete storage failure and the cost of space for multiple full backups is much higher, most organisations choose to *implement incremental backups, typically with a full backup on a periodic basis*.

The ability to recover form backups as well as the organisation's need for that recovery process drive both design decisions and organisational processes. Organisations will create recovery point objectives ([[Recovery Point Objectives|RPOs]]) and recovery time objectives ([[Recovery Time Objectives|RTOs]]) that determine how significant much data loss, if any, is acceptable and how long recovery can take without causing significant damage to the organisation.
### Replication

Replication focuses on using either *synchronous or asynchronous methods to copy live data to another location* or device. Unlike backups that occur periodically, replication is always occurring as changes are made.

Replication helps with multisite, multi-system designs, ensuring that changes are carried over to all systems or clusters that are part of an architecture. Replication can help with disaster recovery, availability, and load balancing.

### Journaling

Another data protection option is journaling, which *creates a log of changes that can be reapplied* if an issue occurs. Journaling is commonly used for **databases** and similar technologies that combine frequent changes with an ability to restore to a point in time.

Journaling also has a role to play in virtual environments where journal-based solutions allow *virtual machines* to be restored to a point in time rather than to a fixed snapshot.

### Snapshots

A snapshot captures the full state of a system or device at the time the backup is completed. Snapshots are common for virtual machines, where they allow the machine state to be restored at the point in time that the snapshot was taken.

Snapshots can be useful to *clone systems*, to go *back in time* to a point before a patch or upgrade was installed, or to restore a system state to a point before some event occurred. Since they are taken live, they can be captured while the system is running, often without significant impact to performance.

Like a full backup, snapshot can consume quite a bit of space, but most virtualisation systems that perform enterprise snapshots are equipped with *compression and deduplication technology* that helps to *optimise space usage* for snapshots.

### Images

Images are a similar concept to snapshots but most often they refer to a complete copy of a system or server, typically down to the bit level for the drive. Images are a backup method of choice for *servers where complex configurations* may be in use, and where cloning or restoration in a short time frame may be desired. 

### VDI

Virtualisation systems and virtual desktop infrastructure (VDI) also use images to *create non-persistent systems*, which are run using a “gold master” image. The gold master image is not modified when the non-persistent system is shut down, thus ensuring that the next user has the same expected experience.


### [[Backup Media]]