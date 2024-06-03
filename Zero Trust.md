Organisations are increasingly designing their networks and infrastructure using Zero Trust principles. Unlike traditional "**moat and castle**" or **defense-in-depth** designs, *Zero Trust presumes that there is no trust boundary and no network edge*. Instead, *each action is validated when requested as part of a continuous authentication process* and access is only allowed after policies are checked, including elements like identity, permissions, system configuration and security status, threat intelligence data review, and security posture.

Figure below shows NIST's logical diagram of a Zero Trust architecture (ZTA). Note subject's use of a system that is untrusted connects through a Policy Enforcement Point, allowing trusted transactions to the enterprise resources. The *Policy Engine makes policy decisions* based on rules that are *then acted on by Policy Administrators*.

![[NIST Zero Trust.jpg]]

In the NIST model:

- Subjects are the users, services, or systems that request access or attempt to use rights.
  
- Policy Engines make policy decisions based on both rules and external systems like those shown in Figure 12.1: threat intelligence, identity management, and SIEM devices, to name a few. They use a trust algorithm that makes the decision to grant, deny, or revoke access to a given resource based on the factors used for input to the algorithm. Once a decision is made, it is logged and the policy administrator takes action based on the decision.
  
- Policy Administrators are not individuals. Rather they are components that establish or remove the communication path between subjects and resources, including creating session-specific authentication tokens or credentials as needed. In cases where access is denied, the policy administrator tells the policy enforcement point to end the session or connection.