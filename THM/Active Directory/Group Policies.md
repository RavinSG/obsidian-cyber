
We have organised users and computers in OUs just for the sake of it, but the main idea behind this is to be able to deploy *different policies for each OU individually*. That way, we can push different configurations and security baselines to users depending on their department.

Windows manages such policies through **Group Policy Objects** (GPO). GPOs are simply a collection of settings that can be applied to OUs. GPOs can contain *policies aimed at either users or computers*, allowing we to set a baseline on specific machines and identities

To configure GPOs, we can use the *Group Policy Management tool*, available from the start menu.

The first thing we will see when opening it is our complete OU hierarchy, as defined before. To configure Group Policies, we first create a GPO under Group Policy Objects and then link it to the OU where we want the policies to apply. As an example, we can see there are some already existing GPOs in our machine:

![[Group Policy Management.png]]

We can see in the image above that 3 GPOs have been created. From those, the `Default Domain Policy` and `RDP Policy` are linked to the `thm.local` domain as a whole, and the `Default Domain Controllers Policy` is linked to the `Domain Controllers` OU only. 

Something important to have in mind is that any **GPO will apply to the linked OU and any sub-OUs** under it. For example, the `Sales` OU will still be affected by the `Default Domain Policy`.

Let's examine the `Default Domain Policy` to see what's inside a GPO. The first tab we'll see when selecting a GPO shows its **scope**, which is where the GPO is linked in the AD. For the current policy, we can see that it has only been linked to the `thm.local` domain:

![[Default Domain Policy.png]]

As we can see, we can also apply **Security Filtering** to GPOs so that they are *only applied to specific users/computers* under an OU. *By default*, they will apply to the Authenticated Users group, which includes *all users/PCs*.

The **Settings** tab includes the actual contents of the GPO and lets us know what specific configurations it applies. As stated before, each GPO has configurations that apply to computers only and configurations that apply to users only. In this case, the `Default Domain Policy` only contains Computer Configurations:

![[GPO Settings.png]]

Since this GPO applies to the whole domain, any change to it would affect all computers. Let's change the minimum password length policy to require users to have at least 10 characters in their passwords. To do this, right-click the GPO and select Edit.

This will open a new window where we can navigate and edit all the available configurations. To change the minimum password length, go to `Computer Configurations -> Policies -> Windows Setting -> Security Settings -> Account Policies -> Password Policy` and change the required policy value:

![[Change Password Policy.png]]

As we can see, plenty of policies can be established in a GPO. If more information on any of the policies is needed, we can double-click them and read the **Explain** tab on each of them.

### GPO Distribution

GPOs are distributed to the network via a network share called `SYSVOL`, which is stored in the DC. All users in a domain should typically have access to this share over the network to sync their GPOs periodically. 

The SYSVOL share points by default to the `C:\Windows\SYSVOL\sysvol\ `directory on each of the [[Domain Controller|DCs]] in our network.

Once a change has been made to any GPOs, it *might take up to 2 hours* for computers to catch up. If you want to force any particular computer to sync its GPOs immediately, you can always run the following command on the desired computer:

```PowerShell
PS C:\> gpupdate /force
```
