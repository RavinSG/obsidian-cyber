---
aliases:
  - AD DS
---
The core of any Windows Domain is the Active Directory Domain Service (**AD DS**). This service acts as a catalogue that holds the information of all of the "objects" that exist on your network. Amongst the many objects supported by [[THM/Active Directory/Active Directory|AD]], we have users, groups, machines, printers, shares and many others. Let's look at some of them:

### Users

Users are one of the most common object types in Active Directory. Users are one of the objects known as **security principals**, meaning that they *can be authenticated by the domain and can be assigned privileges over resources* like files or printers. You could say that a security principal is an object that can act upon resources in the network.

Users can be used to represent two types of entities:

- **People**: users will generally represent persons in your organisation that need to access the network, like employees.
- **Services**: you can also define users to be used by services like IIS or MSSQL. Every single service requires a user to run, but service users are different from regular users as they will only have the privileges needed to run their specific service.

### Machines

Machines are another type of object within Active Directory; *for every computer* that joins the Active Directory domain, a *machine object will be created*. Machines are also considered **security principals** and are assigned an account just as any regular user. This account has somewhat limited rights within the domain itself.

The *machine accounts themselves are local administrators on the assigned computer*, they are generally not supposed to be accessed by anyone except the computer itself, but as with any other account, if you have the password, you can use it to log in.

Note: Machine Account passwords are automatically rotated out and are generally comprised of 120 random characters.

Identifying machine accounts is relatively easy. They follow a specific naming scheme. The machine account name is the computer's name followed by a dollar sign. For example, a machine named `DC01` will have a machine account called `DC01$`.

### Security Groups

Windows can *define user groups to assign access rights to files or other resources* to entire groups instead of single users. This allows for better manageability as you can add users to an existing group, and they will automatically inherit all of the group's privileges. Security groups are also considered **security principals** and, therefore, can have privileges over resources on the network.

Groups can *have both users and machines* as members. If needed, groups can *include other groups as well*.

Several groups are created by default in a domain that can be used to grant specific privileges to users. As an example, here are some of the most important groups in a domain:

| Security Group     | Description                                                                                                                                               |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Domain Admins      | Users of this group have administrative privileges over the entire domain. By default, they can administer any computer on the domain, including the DCs. |
| Server Operators   | Users in this group can administer Domain Controllers. They cannot change any administrative group memberships.                                           |
| Backup Operators   | Users in this group are allowed to access any file, ignoring their permissions. They are used to perform backups of data on computers.                    |
| Account Operators  | Users in this group can create or modify other accounts in the domain.                                                                                    |
| Domain Users       | Includes all existing user accounts in the domain.                                                                                                        |
| Domain Computers   | Includes all existing computers in the domain.                                                                                                            |
| Domain Controllers | Includes all existing DCs on the domain.                                                                                                                  |

