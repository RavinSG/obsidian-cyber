
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

### Replication
