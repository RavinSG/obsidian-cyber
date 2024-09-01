---
aliases:
  - SIEM
---
A **SIEM tool** is an application that collects and analyses [[Security Logs|log data]] to monitor critical activities in an organisation. 

**A log** is a record of events that occur within an organisation’s systems. Depending on the amount of data you’re working with, it could take hours or days to filter through log data on your own. 

SIEM tools reduce the amount of data an analyst must review by providing alerts for specific types of [[Threats|threats]], [[Risks|risks]], and [[Vulnerabilities|vulnerabilities]].

SIEM tools provide a series of *dashboards* that visually organise data into categories, allowing users to select the data they wish to analyse. Different SIEM tools have different dashboard types that display the information you have access to.

SIEM tools also come with different hosting options, including *on-premise and cloud*. Organisations may choose one hosting option over another based on a security team member’s expertise. For example, because a cloud-hosted version tends to be easier to set up, use, and maintain than an on-premise version, a less experienced security team may choose this option for their organisation.

### The future of SIEM tools

As cybersecurity continues to evolve, the need for cloud functionality has increased. SIEM tools have and continue to evolve to function in *cloud-hosted* and *cloud-native* environments. 

**Cloud-hosted** SIEM tools are operated by vendors who are responsible for maintaining and managing the infrastructure required to use the tools. Cloud-hosted tools are simply accessed through the internet and are an ideal solution for organisations that don’t want to invest in creating and maintaining their own infrastructure.

**Cloud-native** SIEM tools are also fully maintained and managed by vendors and accessed through the internet. However, cloud-native tools are designed to take full advantage of cloud computing capabilities, such as availability, flexibility, and scalability.

### Different types of SIEM tools

- Self-hosted
- Cloud-hosted
- Hybrid

### SIEM advantages

SIEM tools collect and manage security-relevant data that can be used during investigations. This is important because SIEM tools provide awareness about the activity that occurs between devices on a network. The information SIEM tools provide can help security teams quickly investigate and respond to security incidents. SIEM tools have many advantages that can help security teams effectively respond to and manage incidents. Some of the advantages are:

- *Access to event data*: SIEM tools provide access to the event and activity data that happens on a network, including real-time activity. Networks can be connected to hundreds of different systems and devices. SIEM tools have the ability to ingest all of this data so that it can be accessed.

- *Monitoring, detecting, and alerting*: SIEM tools continuously monitor systems and networks in real-time. They then analyse the collected data using detection rules to detect malicious activity. If an activity matches the rule, an alert is generated and sent out for security teams to assess.

- *Log storage*: SIEM tools can act as a system for data retention, which can provide access to historical data. Data can be kept or deleted after a period depending on an organisation's requirements.

### The SIEM process

The SIEM process consists of three critical steps:

- Collect and aggregate data
- Normalize data 
- Analyse data

By understanding these steps, organisations can utilise the power of SIEM tools to gather, organise, and analyse security event data from different sources. Organisations can later use this information to improve their ability to identify and mitigate potential threats.

#### Collect and aggregate data

SIEM tools require data for them to be effectively used. During the first step, the SIEM collects event data from *various sources like firewalls, servers, routers, and more*. This data, also known as logs, contains event details like *timestamps, IP addresses, and more*. Logs are a record of events that occur within an organisation’s systems. After all of this log data is collected, it gets *aggregated in one location*. 

Aggregation refers to the process of *consolidating log data into a centralised place*. Through collection and aggregation, SIEM tools *eliminate the need for manually reviewing and analysing* event data by accessing individual data sources. Instead, all event data is accessible in one location—the **SIEM**. 

Parsing can occur during the first step of the SIEM process when data is collected and aggregated. Parsing maps data according to their fields and their corresponding values. For example, the following log example contains fields with values. At first, it might be difficult to interpret information from this log based on its format:

```
April 3 11:01:21 server sshd[1088]: Failed password for user nuhara from 218.124.14.105 port 5023
```

In a parsed format, the fields and values are extracted and paired making them easier to read and interpret:

- host = `server`
- process = `sshd`
- source_user = `nuhara`
- source ip = `218.124.14.105`
- source port = `5023`

#### Normalize data

SIEM tools collect data from many different sources. This data must be *transformed into a single format* so that it can be easily processed by the SIEM. However, e*ach data source is different and data can be formatted in many different ways*. For example, a firewall log can be formatted differently than a server log.

Collected event data should go through the process of normalisation. Normalisation **converts data into a standard, structured format that is easily searchable**. 

#### Analyse data

After log data has been collected, aggregated, and normalised, the SIEM must do something useful with all of the data to enable security teams to investigate threats. During this final step in the process, SIEM tools analyse the data. *Analysis can be done with some type of detection logic such as a set of rules and conditions*. SIEM tools then apply these rules to the data, and if any of the log activity matches a rule, alerts are sent out to cybersecurity teams.

### SIEM Dashboards

#### Sensitivity and Thresholds

Analysts need to understand how to control and limit the alerts that a SIEM can generate. To do that, they set *thresholds, filter rules, and use other methods* of managing the sensitivity of the SIEM. Alerts may be set to activate only when an event *has happened a certain number of times*, or when it *impacts specific high-value systems*. Or, an alert may be set to *activate once instead of hundreds or thousands of times*.

#### Trends

A trend *can point to a new problem that is starting to crop up*, an exploit that is occurring and taking over, or simply which malware is most prevalent in your organisation. Below is an example of categorising malware activity, identifying which signatures have been detected most frequently, which malware family is most prevalent, and where it sends traffic to. This can help organisations identify new threats as they rise to the top.

![[SIEM Dashboard Trend.png]]

#### Alerts and Alarms

Alarms are *categorised by their time and severity*, and then provide detailed information that can be drilled down into. Events like **malware beaconing** and **infection** are *automatically categorised, prioritised, marked by source and destination*, and matched to an investigation by an analyst as appropriate. They also show things like which sensor is reporting the issue.

An important activity for security professionals is **alert tuning**, the process of *modifying alerts to only alarm on important events*. Alerts that are not properly tuned can cause additional work, often alerting responders over and over until they begin to be ignored. 

Properly tuning alerts is a key part of using alerts and alarms so that responders know that the events they will be notified about are worth responding to.
Alert tuning often *involves setting thresholds, removing noise* by identifying false alarms and normal behaviours, and ensuring that tuning is *not overly broad* so that it ignores actual issues and malicious activity.

>[!note] Alert Fatigue
>One of the biggest threats to SIEM deployments is **alert fatigue**. Alert fatigue occurs when alerts are sent *so often, for so many events*, that *analysts stop responding* to them. In most cases, these alerts aren't critical, high urgency, or high impact and are in essence just creating noise. Or, there may be a *very high proportion of false positives*, causing the analyst to spend hours chasing ghosts. 
>
>In either case, alert fatigue means that when *an actual event occurs it may be missed* or simply disregarded, resulting in a much worse security incident than if analysts had been ready and willing to handle it sooner. That's why alert tuning is so important.

#### Log Aggregation, Correlation, and Analysis

*Individual data points* can be **useful when investigating an incident**, but *matching data points* to other data points is a **key part of most investigations**. Correlation requires having data such as the time that an event occurred, what system or systems it occurred on, what user accounts were involved, and other details that can help with the analysis process. 

A SIEM can allow you to *search and filter data* based on multiple data points like these to narrow down the information related to an incident. *Automated correlation and analysis* is designed to match known events and indicators of compromise to build a complete dataset for an incident or event that can then be reviewed and analysed. As you can see in the figure below, from the AlienVault SIEM, you can add tags and investigations to data. Although each SIEM tool may refer to these by slightly different terms, the basic concepts and capabilities remain the same.

![[AlienVault SIEM.png]]

Log aggregation *isn't only done with SIEM devices* or services, however. Centralised logging tools like **syslog-ng**, **rsyslog**, and similar tools provide the ability to centralise logs and perform analysis of the log data. As with many security tools, many solutions exist to gather and analyse logs, with the lines blurring between various solutions like SIEM, [[Security Orchestration, Automation, and Response|SOAR]] (Security Orchestration, Automation, and Response systems), and other security response and analysis tools.

#### Rules

The **heart of alarms, alerts, and correlation engines** for a SIEM is the set of rules that drive those components. Below an example of how an alarm rule can be built using information the SIEM gathers. Rule conditions can use logic to determine if and when a rule will be activated, and then actions can trigger based on the rule. Results may be as simple as an alert or as complex as a programmatic action that changes infrastructure, enables or disables firewall rules, or triggers other defences.

![[SIEM Rule Creation.png]]

*Rules are important but can also cause issues*. Poorly constructed rule logic may miss events or cause false positives or overly broad detections. If the rule has an active response component, a *mis-triggered rule can cause an outage* or other infrastructure issue. Thus, rules need to be carefully built, tested, and reviewed on a regular basis. Although SIEM vendors often provide default rules and detection capabilities, the custom-built rules that organisations design for their environments and systems are key to a successful SIEM deployment.

Finally, *SIEM devices also follow the entire life cycle for data*. That means most have the ability to set retention and data lifespan for each type of data and have support for compliance requirements. In fact, most SIEM devices have prebuilt rules or modules designed to meet specific compliance requirements based on the standards they require.

#### Log Files

Log files provide incident responders with information about what has occurred. Of course, that makes **log files a target for attackers as well**, so incident responders need to *make sure* that the *logs* they are using *have not been tampered with* and that they have time stamp and other data that is correct. Once you're sure the data you are working with is good, logs can provide a treasure trove of incident-related information.

Windows Event Viewer, one of the most common ways to view logs for a single Windows system. In many enterprise environments, specific logs or critical log entries will be sent to a secure logging infrastructure to ensure a trustworthy replica of the logs collected at endpoint systems exists. Security practitioners will still review local logs, particularly because the volume of log data at endpoints throughout an organization means that complete copies of all logs for every system are not typically maintained.

Common logs used by incident responders that are covered in the Security+ exam outline include the following:

- **Firewall logs**, which can provide information about blocked and allowed traffic, and with more advanced firewalls like NGFW or UTM, devices can also provide application-layer details or IDS/IPS functionality along with other security service–related log information.

- **Application logs** for Windows include information like installer information for applications, errors generated by applications, license checks, and any other logs that applications generate and send to the application log. Web servers and other devices also generate logs like those from Apache and Internet Information Services (IIS), which track requests to the web server and related events. These logs can help track what was accessed, when it was accessed, and what IP address sent the request. Since requests are logged, these logs can also help identify attacks, including SQL injection (SQLi) and other web server and web application–specific attacks.

- **Endpoint logs** such as application installation logs, system and service logs, and any other logs available from endpoint systems and devices.

- **OS-specific security logs** for Windows systems store information about failed and successful logins, as well as other authentication log information. Authentication and security logs for Linux systems are stored in /var/log/auth.log and /var/log/secure.

- **IDS/IPS logs** provide insight into attack traffic that was detected or, in the case of IPS, blocked.

- **Network logs** can include logs for routers and switches with configuration changes, traffic information, network flows, and data captured by packet analysers like Wireshark.