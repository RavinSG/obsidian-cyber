
Once you've acquired your forensic data, you need to make sure that you have a complete, accurate copy before you begin forensic analysis. At the same time, *documenting the provenance of the data* and *ensuring that the data and process cannot be repudiated* (nonrepudiation) are also important.

The most common way to validate that a forensic copy matches an original copy is to create a hash of the copy as well as a hash of the original drive, and then compare them. If the hashes match, the forensic copy is identical to the original. Although MD5 and SHA1 are both largely outmoded for purposes where attackers might be involved, they remain useful for quickly hashing forensic images. 

Providing an MD5 or SHA1 hash of both drives, along with documentation of the process and procedures used, is a common part of building the provenance of the copy. *The hashes and other related information will be stored as part of the chain-of-custody and forensic documentation* for the case.

Manually creating a hash of an image file or drive is as simple as pointing the hashing tool to it. Here are examples of a hash for a drive mounted as `/dev/sdb` on a Linux system and an image file in the current directory. The filename selected for output is `drive1.hash`, but it could be any filename you choose.

```bash
md5sum /dev/sdb > drive1.hash
```

or

```bash
md5sum image_file.img > drive1.hash
```

>[!note] Forensic Copies vs. Logical Copies
>Simply copying a file, folder, or drive will result in a *logical copy*. The *data will be preserved, but it will not exactly match the state of the drive or device it was copied from*. When you conduct forensic analysis, it is important to preserve the full content of the drive at a bit-by-bit level, preserving the exact structure of the drive with deleted file remnants, metadata, and time stamps. Forensic copies are therefore done differently than logical copies. Hashing a file may match, but *hashing a logical copy and a forensic copy will provide different values, thus making logical copies inadmissible in many situations* where forensic analysis may involve legal action or unusable when changes to the drive or metadata and deleted files are critical to the investigation.

