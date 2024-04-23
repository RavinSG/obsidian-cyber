
You have been given the following organisational chart and are expected to make changes to the AD to match it:

![[Organisational Chart.png]]

### Deleting extra OUs and users

If we try to delete an OU, we get the following error, 

![[Deleting OUs.png]]

By default, OUs are protected against accidental deletion. To delete the OU, we need to enable the **Advanced Features** in the View menu. 

This will show you some additional containers and enable you to disable the accidental deletion protection. To do so, right-click the OU and go to Properties. You will find a checkbox in the Object tab to *disable the protection*.

![[Screenshot 2024-04-23 at 12.23.46 PM.png|500]]


Be sure to uncheck the box and try deleting the OU again. You will be prompted to confirm that you want to delete the OU, and as a result, any users, groups or OUs under it will also be deleted.

### Delegation

One of the nice things you can do in AD is to give *specific users some control over some OUs*. This process is known as **delegation** and allows you to grant users specific privileges to perform advanced tasks on OUs *without needing a Domain Administrator* to step in.

One of the most common use cases for this is granting IT support the privileges to reset other low-privilege users' passwords. According to our organisational chart, Phillip is in charge of IT support, so we'd probably want to delegate the control of resetting passwords over the Sales, Marketing and Management OUs to him.

For this example, we will delegate control over the Sales OU to Phillip. To delegate control over an OU, you can right-click it and select Delegate Control:

![[Delegation.png]]

Following the instructions we can delegate the ability to Phillip to reset the passwords of other users. However, while we may be tempted to go to **Active Directory Users and Computers** to try and test *Phillip*'s new powers, he *doesn't really have the privileges to open it*, so you'll have to use other methods to do password resets. In this case, we will be using Powershell to do so:

```PowerShell
PS C:\Users\phillip> Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose

New Password: *********

VERBOSE: Performing the operation "Set-ADAccountPassword" on target "CN=Sophie,OU=Sales,OU=THM,DC=thm,DC=local".
```

```PowerShell
PS C:\Users\phillip> Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose

VERBOSE: Performing the operation "Set" on target "CN=Sophie,OU=Sales,OU=THM,DC=thm,DC=local".
```

