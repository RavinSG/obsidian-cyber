If the right user has been authenticated, the network should ensure the *right resources are made available*. There are three common frameworks that organisations use to handle this step of IAM:

- [[Mandatory Access Control]] (**MAC**)
- [[Discretionary Access Control]] (**DAC**)
- [[Role-Based Access Control]] (**RBAC**)
- [[Rule-Based Access Control]] (**RuBAC**)
- [[Attribute-Based Access Control]] (**ABAC**)

In addition to the above, there are other concepts under access control:

- *Time-of-day restrictions*, which limit when activities can occur. An example in Windows is where logon hours can be set via Active Directory, defining the hours that a user or group of users can be logged onto a system.
  
- *Least privilege* is a common concept throughout information security practices and should be designed into any permission or access scheme.
### Access control technologies

Users often experience **authentication** and **authorisation** as a single, seamless experience. In large part, that’s due to access control technologies that are configured to work together. These tools offer the speed and automation needed by administrators to monitor and modify access rights. They also decrease errors and potential risks.

An organisation's IT department sometimes develops and maintains customised access control technologies on their own. A typical **IAM** or **AAA** system consists of a user directory, a set of tools for managing data in that directory, an authorisation system, and an auditing system. Some organisations create custom systems to tailor them to their security needs. However, building an in-house solution comes at a steep cost of time and other resources.

Instead, many organisations opt to license third-party solutions that offer a suite of tools that enable them to quickly secure their information systems. Keep in mind, security is about more than combining a bunch of tools. It’s always important to configure these technologies so they can help to provide a secure environment.

### Filesystem Permissions

Filesystem controls determine which accounts, users, groups, or services can perform actions like reading, writing, and executing files. 

**Linux** filesystem permissions are shown in file listings with the letters `drwxrwxrwx`, indicating whether a file is a directory, and then displaying user, group, and world (sometimes called other) permissions.

The figure below shows how this is displayed and a chart describing the numeric representation of these settings that is frequently used for shorthand when using the chmod Linux command used to change permissions.

![[Linux File Permissions.png]]

**Windows** file permissions can be set using the command line or the GUI. Below are the properties of a file using the GUI. Note that the permissions are similar but not quite the same as those set in Linux.

![[Windows File Permissions.png]]



