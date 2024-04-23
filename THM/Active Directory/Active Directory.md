---
aliases:
  - AD
---

Microsoft's Active Directory is the backbone of the corporate world. It simplifies the management of devices and users within a corporate environment.

A **Windows domain** is a group of users and computers under the administration of a given business. The main idea behind a domain is to centralise the administration of common components of a Windows computer network in a single repository called **Active Directory** (AD). The server that runs the Active Directory services is known as a **Domain Controller** (DC).

![[Windows Domain.png]]

The main advantages of having a configured Windows domain are:

- *Centralised identity management*: All users across the network can be configured from Active Directory with minimum effort.
- *Managing security policies*: You can configure security policies directly from Active Directory and apply them to users and computers across the network as needed.

### A Real-World Example

In school/university networks, you will often be provided with a username and password that you can use on any of the computers available on campus. Your *credentials are valid for all machines* because whenever you input them on a machine, it will forward the authentication process back to the Active Directory, where your credentials will be checked. Thanks to Active Directory, your credentials don't need to exist in each machine and are *available throughout the network*.

Active Directory is also the component that allows your school/university to *restrict you from accessing the control panel* on your school/university machines. Policies will usually be deployed throughout the network so that you don't have administrative privileges over those computers.

### Active Directory Users and Computers

To configure users, groups or machines in Active Directory, we need to log in to the Domain Controller and run "Active Directory Users and Computers" from the start menu:

![[AD Users.png]]

This will open up a window where you can see the *hierarchy of users*, computers and groups that exist in the domain. These objects are organised in **Organisational Units** (OUs) which are container objects that allow you to classify users and machines. OUs are mainly used to define sets of users with similar policing requirements. 

The people in the Sales department of your organisation are likely to have a different set of policies applied than the people in IT, for example. Keep in mind that a **user can only be a part of a single OU at a time**.

Checking our machine, we can see that there is already an **OU** called `THM` with *four child OUs* for the IT, Management, Marketing and Sales departments. It is very typical to see the OUs mimic the business' structure, as it allows for efficiently deploying baseline policies that apply to entire departments. Remember that while this would be the expected model most of the time, you can define OUs arbitrarily.

![[AD OUs.png]]


You probably noticed already that there are other *default containers* apart from the THM OU. These containers are created by Windows automatically and contain the following:

- **Builtin**: Contains default groups available to any Windows host.
- **Computers**: Any machine joining the network will be put here by default. You can move them if needed.
- **Domain Controllers**: Default OU that contains the DCs in your network.
- **Users**: Default users and groups that apply to a domain-wide context.
- **Managed** Service Accounts: Holds accounts used by services in your Windows domain.

### Security Groups vs OUs

You are probably wondering why we have both groups and OUs. While both are used to classify users and computers, their purposes are entirely different:

- **OUs** are handy for *applying policies to users and computers*, which include specific configurations that pertain to sets of users depending on their particular role in the enterprise. Remember, a user can only be a member of a single OU at a time, as it wouldn't make sense to try to apply two different sets of policies to a single user.
  
- **Security Groups**, on the other hand, are *used to grant permissions over resources*. For example, you will use groups if you want to allow some users to access a shared folder or network printer. A user can be a part of many groups, which is needed to grant access to multiple resources.
