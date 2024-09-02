
Traditional Linux logs are sent via **syslog**, with clients sending messages to servers that collect and store the logs. Over time, other syslog replacements have been created to improve upon the basic functionality and capabilities of syslog. *When speed is necessary*, the rocket-fast system for log processing, or **rsyslog**, is an option. It supports *extremely high message rates*, *secure logging via TLS*, and *TCP-based messages* as well as *multiple backend database options*.

Another alternative is **syslog-ng**, which provides enhanced filtering, direct logging to databases, and support for sending logs via TCP protected by TLS. The enhanced features of syslog replacements like rsyslog and syslog-ng mean that many organisations replace their syslog infrastructure with one of these options. 

A final option for log collection is **NXLog**, an *open source* and commercially supported syslog centralisation and aggregation tool that can parse and generate log files in many common formats while also sending logs to analysis tools and SIEM solutions.

>[!info] Digging Into systemd's Journal in Linux
>
>Most Linux distributions rely on **systemd** to *manage services and processes* and, in general, manage the system itself. Accessing the systemd journal that records what systemd is doing using the journald daemon can be accomplished using journalctl. This tool allows you to review kernel, services, and initrd messages as well as many others that systemd generates. Simply issuing the journalctl command will display all the journal entries, but additional modes can be useful. If you need to see what happened since the last boot, the -b flag will show only those entries. Filtering by time can be accomplished with the --since flag and a time/date “entry in the format “year-month-day hour:minute:seconds"


Regardless of the logging system you use, you will have to *make decisions about retention on both local systems* and *central logging and monitoring infrastructure*. Take into account operational needs; likely scenarios where you may need the logs you collect; and **legal, compliance**, or other requirements that you need to meet. 

In many cases organisations choose to keep logs for 30, 45, 90, or 180 days depending on their needs, but some cases may even result in some logs being kept for a year or more. *Retention comes with both hardware costs and potential legal challenges* if you retain logs that you may not wish to disclose in court.

### Going Beyond Logs: Using Metadata

Log entries aren't the only useful data that systems contain. *Metadata* generated as a normal part of system operations, communications, and other activities can also be *used for incident response*. Metadata is data about other data—in the case of systems and services, metadata is created as part of files, embedded in documents, used to define structured data, and included in transactions and network communications, among many other places you can find it.

Four common examples of metadata are:

- **Email** metadata includes headers and other information found in an email. *Email headers* provide details about the sender, the recipient, the date and time the message was sent, whether the email had an attachment, *which systems the email traveled through*, and other header markup that systems may have added, including antispam and other information.

- **Mobile** metadata is collected by phones and other mobile devices as they are used. It can include *call logs, SMS and other message data, data usage, GPS location tracking, cellular tower information*, and other details found in call data records. Mobile metadata is **incredibly powerful** because of the amount of geospatial information that is recorded about where the phone is at any point during each day.

- **Web** metadata is embedded into websites as part of the code of the website but is often invisible to everyday users. It can include *metatags, headers, cookies*, and other information that help with search engine optimisation, website functionality, advertising, and tracking, or that may support specific functionality.

- **File** metadata can be a powerful tool when reviewing when a file was created, how it was created, if and when it was modified, who modified it, the GPS location of the device that created it, and many other details. The code below shows selected metadata recovered from a single photo using [ExifTool](http://exiftool.org). The output shows that the photo was taken with a digital camera, which inserted metadata such as the date the photo was taken, the specific camera that took it, the camera's settings, and even the firmware version. Mobile devices may also include the GPS location of the photo if they are not set to remove that information from photos, resulting in even more information leakage.
`
```
File Size                    : 2.0 MB
File Modification Date/Time  : 2009:11:28 14:36:02-05:00
Make                         : Canon
Camera Model Name            : Canon PowerShot A610
Orientation                  : Horizontal (normal)
X Resolution                 : 180
Y Resolution                 : 180
Resolution Unit              : inches
Modify Date                  : 2009:08:22 14:52:16
Exposure Time                : 1/400
F Number                     : 4.0
Date/Time Original           : 2009:08:22 14:52:16
Create Date                  : 2009:08:22 14:52:16
Flash                        : Off, Did not fire
Canon Firmware Version       : Firmware Version 1.00
```

Metadata is commonly used for forensic and other investigations, and most forensic tools have built-in metadata-viewing capabilities.

### Other Data Sources

In addition to system and service logs, other data sources can also be used, either through SIEM and *other log and event management systems or manually*. They can be acquired using agents, special-purpose software deployed to systems and devices that send the logs to a log aggregator or management system, or they can be agentless and simply send the logs via standardised log interfaces like syslog.

Other useful data can be found in automated reports from various systems and services and dashboards that are available via management tools and administrative control panels.

### Benchmarks and Logging

You're already familiar with the concept of using benchmarks to configure systems to a known standard security configuration, but *benchmarks also often include log settings*. That means that a well-constructed benchmark might require central logging, configuring log and alerting levels, and that endpoints or servers log critical and important events. As you consider how an organization manages systems, services, and devices at scale, *benchmarks are a useful means of ensuring that each of them is configured to log important information* in useful ways to support security operations.

### Reporting and Archiving

Once you've gathered logs, two key actions remain: **reporting** and **archiving**. *Reporting* on log information is part of the overall log management process, *including identifying trends* and providing visibility into changes in the logs that may indicate issues or require management oversight. Finally, it is important that organisations *consider the full lifespan of their log data*. That includes setting data retention life cycles and archiving logs when they must be retained but are not in active use. This helps to make sure space is available in SIEM and other devices and also keeps the logs to a manageable size for analysis.

