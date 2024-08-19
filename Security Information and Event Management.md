---
aliases:
  - SIEM
---
A **SIEM tool** is an application that collects and analyses [[Security Logs|log data]] to monitor critical activities in an organization. 

**A log** is a record of events that occur within an organisation’s systems. Depending on the amount of data you’re working with, it could take hours or days to filter through log data on your own. 

SIEM tools reduce the amount of data an analyst must review by providing alerts for specific types of [[Threats|threats]], [[Risks|risks]], and [[Vulnerabilities|vulnerabilities]].

SIEM tools provide a series of *dashboards* that visually organise data into categories, allowing users to select the data they wish to analyse. Different SIEM tools have different dashboard types that display the information you have access to.

SIEM tools also come with different hosting options, including *on-premise and cloud*. Organisations may choose one hosting option over another based on a security team member’s expertise. For example, because a cloud-hosted version tends to be easier to set up, use, and maintain than an on-premise version, a less experienced security team may choose this option for their organization.

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

