
Backup media decisions involve **capacity**, **reliability**, **speed**, **cost**, **expected lifespan** while storing data, **how often it can be reused** before wearing out, and other factors, all of which can influence the backup solution that an organization chooses. Common choices include the following:

- **Tape** has historically been one of the lowest-cost-per-capacity options for large-scale backups. Magnetic tape remains in use in large enterprises, often in the form of tape robot systems that can load and store very large numbers of tapes using a few drives and several cartridge storage slots.
  
- **Disks**, either in magnetic or solid-state drive form, are typically more expensive for the same backup capacity as tape but are often faster. Disks are often used in large arrays in either a network attached storage (NAS) device or a storage area network (SAN).
  
- **Optical media** like Blu-ray disks and DVDs, as well as specialised optical storage systems, remain in use in some circumstances, but for capacity reasons they are not in common use as a large scale backup tool.
  
- **Flash media** like microSD cards and USB thumb drives continue to be used in many places for short-term copies and even longer term backups. Though they aren't frequently used at an enterprise scale, they are important to note as a type of media that may be used for some backups.


### Online, Offline, and Nearline

The advantage of *online* backups is in *quick retrieval* and accessibility, whereas *offline* backups can be kept in a secure location *without power and other expense* required for their active maintenance.

Offline backups are often used to *ensure that an organization cannot have a total data loss*, whereas online backups help you respond to immediate issues and maintain operations.

"**Nearline**” backups—backup storage that is not immediately available but that can be *retrieved within a reasonable period of time*, usually without a human involved. Tape robots are a common example of nearline storage, with backup tapes accessed and their contents provided on demand by the robot.

Cloud backups like Amazon's Glacier and Google's Coldline provide lower prices for slower access times and provide what is essentially offline storage with a nearline access model.

### Off-site

Some organisations choose to utilise off-site storage for their backup media, either at a site they own and operate or through a third-party service like Iron Mountain, which specialises in storage of secure backups in environmentally controlled facilities. Off-site storage, a form of **geographic diversity**, helps ensure that a single disaster
cannot destroy an organisation's data entirely.

Cloud and third-party off-site backup options have continued to become increasingly common. A few important considerations come into play with cloud and off-site third-party backup options:

- *Bandwidth requirements* for both the backups themselves and restoration time if the backup needs to be restored partially or fully. Low bandwidths make off-site options less attractive if quick restoration is required, but they remain attractive from a disaster recovery perspective to ensure that data is not lost completely.
  
- *Time and cost to retrieve files*. Solutions like Amazon's Glacier storage focus on low-cost storage but have higher costs for retrieval, as well as slower retrieval times.
  
- *Reliability*. Many cloud providers have extremely high advertised reliability rates for their backup and storage services, and these rates may actually beat the expected durability of local tape or disk options.
  
- *New security models* required for backups. Separation of accounts, additional controls, and encryption of data in the remote storage location are all common considerations for use of third-party services.

Regardless of the type of backup you select, securing the backup when it is in storage and in transit using **encryption** is an import consideration. Backups are commonly encrypted at rest, with encryption keys requiring for restoration, and are also protected by transport layer encryption when they are transferred across a network.