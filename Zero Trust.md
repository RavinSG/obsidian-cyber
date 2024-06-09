Organisations are increasingly designing their networks and infrastructure using Zero Trust principles. Unlike traditional "**moat and castle**" or **defense-in-depth** designs, *Zero Trust presumes that there is no trust boundary and no network edge*. Instead, *each action is validated when requested as part of a continuous authentication process* and access is only allowed after policies are checked, including elements like identity, permissions, system configuration and security status, threat intelligence data review, and security posture.

Figure below shows NIST's logical diagram of a Zero Trust architecture (ZTA). Note subject's use of a system that is untrusted connects through a Policy Enforcement Point, allowing trusted transactions to the enterprise resources. The *Policy Engine makes policy decisions* based on rules that are *then acted on by Policy Administrators*.

![[NIST Zero Trust.jpg]]

In the NIST model:

- **Subjects** are the users, services, or systems that request access or attempt to use rights.
  
- **Policy Engines** make policy decisions based on both rules and external systems like those shown in the above figure; threat intelligence, identity management, and SIEM devices, to name a few. They use a trust algorithm that makes the decision to grant, deny, or revoke access to a given resource based on the factors used for input to the algorithm. Once a decision is made, it is logged and the policy administrator takes action based on the decision.
  
- **Policy Administrators** are not individuals. Rather they are components that establish or remove the communication path between subjects and resources, including creating session-specific authentication tokens or credentials as needed. In cases where access is denied, the policy administrator tells the policy enforcement point to end the session or connection.
  
- **Policy Enforcement Points** communicate with Policy Administrators to forward requests from subjects and to receive instruction from the policy administrators about connections to allow or end. While the Policy Enforcement Point is shown as a single logical element in the above figure, it is commonly deployed with a local client or application and a gateway element that is part of the network path to services and resources.

There are two major Zero Trust planes that you should be aware of: the *Control Plane* and the *Data Plane*.

### Control Plane

The Control Plane is composed of four components:

- **Adaptive identity** (often called adaptive authentication), which *leverages context-based authentication* that considers data points such as where the user is logging in from, what device they are logging in from, and whether the device meets security and configuration requirements. Adaptive authentication methods may then request additional identity validation if requirements are not met or may decline authentication if policies do not allow for additional validation.
  
- **Threat scope reduction**, sometimes described as "limited blast radius," is a key component in Zero Trust design. *Limiting the scope of what a subject can do* as well or what access is permitted to a resource limits what can go wrong if an issue does occur. Threat scope reduction relies on least privilege as well as identity-based network segmentation that is based on identity rather than tradition network-based segmentation based on things like an IP address, a network segment, or a VLAN.
  
- **Policy-driven access control** is a core concept for Zero Trust. Policy Engines rely on Police as they make decisions that are then enforced by the Policy Administrator and Policy Enforcement Points.
  
- **The Policy Administrator**, which as described in the NIST model, executes decisions made by the policy engine.

### Data Plane

- **Implicit trust zones**, which allow use and movement once a subject is authenticated by a Zero Trust Policy Engine
  
- **Subjects and systems** (subject/system), which are the devices and users that are seeking access
  
- **Policy Enforcement Points**, which match the NIST description