---
aliases:
  - ACL
---
Access control lists are rules that either permit or deny actions. For network devices, they are typically similar to firewall rules. ACLs can be simple or complex, ranging from a single statement to multiple entries that apply to traffic. Network devices may also provide more advanced forms of ACLs, including time-based, dynamic, or other ACLs that have conditions that impact their application.

A sample of what an ACL might contain is shown below;

| Rule Number | Protocol | Ports | Destination   | Allow/Deny | Notes                   |
| ----------- | -------- | ----- | ------------- | ---------- | ----------------------- |
| 10          | TCP      | 22    | 10.0.10.0/24  | ALLOW      | Allow SSH               |
| 20          | TCP      | 443   | 10.0.10.45/32 | ALLOW      | Inbound HTTPs to Server |
| 30          | ICMP     | ALL   | 0.0.0.0/0     | DENY       | Block ICMP              |
Cloud services also provide network ACLs. [[Cloud Infrastructre Components#Virtual Private Cloud (VPC)|VPCs]] and other services provide firewall-like rules that can restrict or allow traffic between systems and services. Like firewall rules, these can typically be grouped, tagged, and managed using security groups or other methods.