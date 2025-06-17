
## System Enumeration

One of the basic commands we can use is `systeminfo`. This will dump out all the basic information we need to know about the machine. Such as, OS version, Architecture, System Model, etc.

```PowerShell
PS C:\Users\fcastle> systeminfo

Host Name:                 THEPUNISHER
OS Name:                   Microsoft Windows 10 Enterprise Evaluation
OS Version:                10.0.19045 N/A Build 19045
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Member Workstation
OS Build Type:             Multiprocessor Free
Registered Owner:          frankcastle
Registered Organization:
Product ID:                00329-20000-00001-AA517
Original Install Date:     13/02/2025, 12:26:50 PM
System Boot Time:          17/06/2025, 6:59:28 PM
System Manufacturer:       VMware, Inc.
System Model:              VMware20,1
System Type:               x64-based PC
.
.
PS C:\Users\fcastle> systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
OS Name:                   Microsoft Windows 10 Enterprise Evaluation
OS Version:                10.0.19045 N/A Build 19045
System Type:               x64-based PC
```

We can also check for installed patches using `wmic` (Windows Management Instrumentation). The `qfe` (Quick Fix Engineering) command will list down the following information. 

```Powershell
PS C:\Users\fcastle> wmic qfe
Caption                                     CSName       Description      FixComments  HotFixID   InstallDate  InstalledBy          InstalledOn  Name  ServicePackInEffect  Status
http://support.microsoft.com/?kbid=5050576  THEPUNISHER  Update                        KB5050576               NT AUTHORITY\SYSTEM  3/4/2025
http://support.microsoft.com/?kbid=5049613  THEPUNISHER  Update                        KB5049613               NT AUTHORITY\SYSTEM  2/13/2025
http://support.microsoft.com/?kbid=5011048  THEPUNISHER  Update                        KB5011048               NT AUTHORITY\SYSTEM  2/13/2025
https://support.microsoft.com/help/5015684  THEPUNISHER  Update                        KB5015684                                    9/8/2022
https://support.microsoft.com/help/5026037  THEPUNISHER  Update                        KB5026037               NT AUTHORITY\SYSTEM  2/13/2025
https://support.microsoft.com/help/5053606  THEPUNISHER  Security Update               KB5053606               NT AUTHORITY\SYSTEM  3/24/2025
```

If we need filtered results, we can use the `get` option and list down the columns we want.

```PowerShell
PS C:\Users\fcastle> wmic qfe get Caption,Description,HotFixID,InstalledOn
Caption                                     Description      HotFixID   InstalledOn
http://support.microsoft.com/?kbid=5050576  Update           KB5050576  3/4/2025
http://support.microsoft.com/?kbid=5049613  Update           KB5049613  2/13/2025
http://support.microsoft.com/?kbid=5011048  Update           KB5011048  2/13/2025
https://support.microsoft.com/help/5015684  Update           KB5015684  9/8/2022
https://support.microsoft.com/help/5026037  Update           KB5026037  2/13/2025
https://support.microsoft.com/help/5053606  Security Update  KB5053606  3/24/2025
```

We can also list down the drives as follow.

```PowerShell
PS C:\Users\fcastle> wmic logicaldisk get Caption,Description,ProviderName
Caption  Description       ProviderName
C:       Local Fixed Disk
D:       CD-ROM Disc
```


## User Enumeration
