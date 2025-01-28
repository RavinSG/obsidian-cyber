
One easy way to check whether there are leaked passwords is to check the user's **history**!

We can search for passwords in plain text files by executing a system-wide `grep` command as follow.

```bash
grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null
```

This will also colour code the text in red for easy identification.

While any user can access the `/etc/passwd` file, only root should be able to read the `/etc/shadow` file. Below is a misconfigured `passwd` file where any user can read the content. This can be abused to crack the hash using a tool like `hashcat` and gain access to root.

```bash
TCM@debian:~$ ls -lt /etc/shadow
-rw-rw-r-- 1 root shadow 809 Jun 17  2020 /etc/shadow
TCM@debian:~$ ls -lt /etc/passwd
-rw-r--r-- 1 root root 950 Jun 17  2020 /etc/passwd
```

In addition, if we can write to either of the files, we can remove the `x` from the `/etc/passwd` file. **If there is no placeholder, it means there's no password**. We can also edit user ids and groups to gain access as well. If we set the user id to 0 for our user, we can become the root.

If we can edit the `/etc/shadow` file, we can replace the hash with a known hash and login with the password. 