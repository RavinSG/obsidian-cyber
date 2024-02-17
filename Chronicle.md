
Chronicle is a cloud-native [[Security Information and Event Management|SIEM]] tool from Google that retains, analyses, and searches log data to identify potential security threats, risks, and vulnerabilities. Chronicle allows you to collect and analyse log data according to: 

- A specific asset 
- A domain name
- A user
- An IP address

Chronicle provides multiple dashboards that help analysts monitor an organisation’s logs, create filters and alerts, and track suspicious domain names. 

#### Enterprise insights dashboard
The enterprise insights dashboard highlights recent alerts. It identifies suspicious domain names in logs, known as indicators of compromise (IOCs). Each result is labeled with a confidence score to indicate the likelihood of a threat. It also provides a severity level that indicates the significance of each threat to the organization. A security analyst might use this dashboard to monitor login or data access attempts related to a critical asset—like an application or system—from unusual locations or devices.

#### Data ingestion and health dashboard
The data ingestion and health dashboard shows the number of event logs, log sources, and success rates of data being processed into Chronicle. A security analyst might use this dashboard to ensure that log sources are correctly configured and that logs are received without error. This helps ensure that log related issues are addressed so that the security team has access to the log data they need.

#### IOC matches dashboard
The IOC matches dashboard indicates the top threats, risks, and vulnerabilities to the organization. Security professionals use this dashboard to observe domain names, IP addresses, and device IOCs over time in order to identify trends. This information is then used to direct the security team’s focus to the highest priority threats. For example, security analysts can use this dashboard to search for additional activity associated with an alert, such as a suspicious user login from an unusual geographic location. 

#### Main dashboard
The main dashboard displays a high-level summary of information related to the organisation’s data ingestion, alerting, and event activity over time. Security professionals can use this dashboard to access a timeline of security events—such as a spike in failed login attempts— to identify threat trends across log sources, devices, IP addresses, and physical locations.

#### Rule detections dashboard
The rule detections dashboard provides statistics related to incidents with the highest occurrences, severities, and detections over time. Security analysts can use this dashboard to access a list of all the alerts triggered by a specific detection rule, such as a rule designed to alert whenever a user opens a known malicious attachment from an email. Analysts then use those statistics to help manage recurring incidents and establish mitigation tactics to reduce an organisation's level of risk.

#### User sign in overview dashboard
The user sign in overview dashboard provides information about user access behaviour across the organization. Security analysts can use this dashboard to access a list of all user sign-in events to identify unusual user activity, such as a user signing in from multiple locations at the same time. This information is then used to help mitigate threats, risks, and vulnerabilities to user accounts and the organisation’s applications.

