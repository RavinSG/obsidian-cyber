
### System Enumeration

Once we have gained access to a machine as a low privilege user, we can do some enumeration to gain information about the machine to search for vulnerabilities.

The `uname` command can give us information about the operating system the machine is using along the architecture. `cat /proc/version` and `cat /etc/issue` can provide more information.

```bash
TCM@debian:~$ uname -a
Linux debian 2.6.32-5-amd64 #1 SMP Tue May 13 16:34:35 UTC 2014 x86_64 GNU/Linux

TCM@debian:~$ cat /proc/version 
Linux version 2.6.32-5-amd64 (Debian 2.6.32-48squeeze6) (jmm@debian.org) (gcc version 4.3.5 (Debian 4.3.5-4) ) #1 SMP Tue May 13 16:34:35 UTC 2014

TCM@debian:~$ cat /etc/issue
Debian GNU/Linux 6.0 \n \l
```

We can learn more about the CPU by issuing the `lscpu` command

```bash
TCM@debian:~$ lscpu 
Architecture:          x86_64
CPU op-mode(s):        64-bit
CPU(s):                1
Thread(s) per core:    1
Core(s) per socket:    1
CPU socket(s):         1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 79
Stepping:              1
CPU MHz:               2300.010
Hypervisor vendor:     Xen
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              46080K
```

### Service Enumeration

We can then run `ps aux` to get information about the services that are running on the machine

```bash
TCM@debian:~$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0   8396   812 ?        Ss   18:01   0:00 init [2]  
root         2  0.0  0.0      0     0 ?        S    18:01   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    18:01   0:00 [migration/0]
root         4  0.0  0.0      0     0 ?        S    18:01   0:00 [ksoftirqd/0]
root         5  0.0  0.0      0     0 ?        S    18:01   0:00 [watchdog/0]
root         6  0.0  0.0      0     0 ?        S    18:01   0:00 [events/0]
root         7  0.0  0.0      0     0 ?        S    18:01   0:00 [cpuset]
.
.
.
```

For example we can get the following information from the output.

Our activity is reflected in the output. Since the services are ordered based on when they started, our activities are at the bottom.

```bash
.
.
TCM       2282  0.0  0.0  76728  1712 ?        S    18:20   0:00 sshd: TCM@pts/0  
TCM       2283  0.0  0.1  19280  2076 pts/0    Ss   18:20   0:00 -bash
TCM       2495  0.0  0.0  16380  1184 pts/0    R+   18:43   0:00 ps aux
```

We can see an `nginx` server is running with `www-data`

```bash
www-data  2027  0.0  0.0  62232  1840 ?        S    18:03   0:00 nginx: worker process
www-data  2028  0.0  0.0  62232  1820 ?        S    18:03   0:00 nginx: worker process
```

We also see `apache` is running on the machine, so there might be a web server available

```bash
www-data  1650  0.0  0.1 294852  2636 ?        Sl   18:03   0:00 /usr/sbin/apache2 -k start
www-data  1651  0.0  0.1 294852  2628 ?        Sl   18:03   0:00 /usr/sbin/apache2 -k start
```

The root is running `cron` jobs

```
root      1749  0.0  0.0  22440   884 ?        Ss   18:03   0:00 /usr/sbin/cron
```

There are network file shares available

```bash
root      1600  0.0  0.0      0     0 ?        S    18:03   0:00 [nfsd4]
root      1601  0.0  0.0      0     0 ?        S    18:03   0:00 [nfsd]
```

### User Enumeration

Next we want to find who we are, what permissions we have, and what we can do. The basic commands to get information about the user are `whoami` and `id`.

```bash
TCM@debian:~$ whoami 
TCM
TCM@debian:~$ id
uid=1000(TCM) gid=1000(user) groups=1000(user),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev)
```

The next thing we can do is check for `sudo` commands that we can run as the user.

```bash
TCM@debian:~$ sudo -l
Matching Defaults entries for TCM on this host:
    env_reset, env_keep+=LD_PRELOAD

User TCM may run the following commands on this host:
    (root) NOPASSWD: /usr/sbin/iftop
    (root) NOPASSWD: /usr/bin/find
    (root) NOPASSWD: /usr/bin/nano
    (root) NOPASSWD: /usr/bin/vim
    (root) NOPASSWD: /usr/bin/man
    (root) NOPASSWD: /usr/bin/awk
    (root) NOPASSWD: /usr/bin/less
    (root) NOPASSWD: /usr/bin/ftp
    (root) NOPASSWD: /usr/bin/nmap
    (root) NOPASSWD: /usr/sbin/apache2
    (root) NOPASSWD: /bin/more
```

An important task is to check whether we can read the `/etc/passwd` and the `/etc/shadow` files.

```bash
TCM@debian:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
.
.

TCM@debian:~$ cat /etc/shadow
root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0:17298:0:99999:7:::
daemon:*:17298:0:99999:7:::
bin:*:17298:0:99999:7::
.
.
```

It seems like this user has access to both files, which is great for us but not for the system. Another file we can check is the `/etc/group` file, which contains user group information

```bash
TCM@debian:~$ cat /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
.
.
```

`history` is another important place we can check to see whether the user has entered some sensitive information in the terminal.

```bash
TCM@debian:~$ history 
    1  ls -al
    2  cat .bash_history 
    3  ls -al
    4  mysql -h somehost.local -uroot -ppassword123
    5  exit
    6  cd /tmp
```

Here we can see the user has entered a `mysql` password as plaintext in the terminal.

### Network Enumeration

The basic command `ip a` would give an overview of the interfaces present in the machine.

We can use the `ip route` command to identify whether there are any routes to other networks.

```bash
TCM@debian:~$ ip route
10.10.0.0/16 dev eth0  proto kernel  scope link  src 10.10.188.206 
default via 10.10.0.1 dev eth0 
```

Another place we can look at is arp tables. This would give us information about any machines our machine has been in contact with.

```bash
TCM@debian:~$ ip neigh
10.10.0.1 dev eth0 lladdr 02:c8:85:b5:5a:aa REACHABLE
```

It seems like this machine is only communicating with us via the default gateway.

Finally we can check whether there are any connections established with our machine via the `netstat` command.

```bash
TCM@debian:~$ netstat -ano
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       Timer
tcp        0      0 0.0.0.0:58477           0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:25              0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:36766           0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:2049            0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:33634           0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0     40 10.10.188.206:22        10.4.72.115:48876       ESTABLISHED on (0.30/0/0)
tcp6       0      0 :::80                   :::*                    LISTEN      off (0.00/0/0)
tcp6       0      0 :::22                   :::*                    LISTEN      off (0.00/0/0)
udp        0      0 0.0.0.0:33840           0.0.0.0:*                           off (0.00/0/0)
udp        0      0 127.0.0.1:688           0.0.0.0:*                           off (0.00/0/0)
udp        0      0 0.0.0.0:68              0.0.0.0:*                           off (0.00/0/0)
```

We can see that there is a port (688) which is only accessible via this machine.

### Password Hunting
