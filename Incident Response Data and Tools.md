
### Monitoring Computing Resources

There are mainly three types of monitoring **systems**, **applications**, and **infrastructure**.

**System monitoring** is *typically done via system logs* as well as through central management tools, including those found in cloud services. System health and performance information may be aggregated and analysed through those management tools in addition to being gathered at central logging servers or services.

**Application monitoring** may *involve application logs, application management interfaces, and performance monitoring tools*. This can vary significantly based on what the application provides, meaning that each application and application environment will need to be analysed and designed to support monitoring.

**Infrastructure** devices can also generate logs. *SNMP and syslog are both commonly used for infrastructure devices*. In addition, hardware vendors often sell management tools and systems that are used to monitor and control infrastructure systems and devices.

This complex set of devices that each generate their own logs and have different log levels and events that may be important *drives the importance of devices like security information and event management* ([[Security Information and Event Management|SIEM]]) devices, which have profiles for each type of device or service and which can correlate and alert on activity based on rules and heuristic analysis.

### [[Logging Protocols and Tools]]