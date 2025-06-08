
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

## SSH Keys

We can check for two types of SSH keys in a machine. Authorised_keys will let us know which users can access this machine. This is the public key that is stored in the `authorized_keys` folder.

```bash
root@debian:~# find / -name authorized_keys 2> /dev/null
/root/.ssh/authorized_keys
root@debian:~# cat /root/.ssh/authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDNJa+p/F4ilO4WZ0AXJYNCGjSHy53eD3NInfamOiw1xNAl7f+RgLgHmEyV8hfMfmgqf1hGfnk7XRFjGmlmlcWxzgfuliq4PlMsouZoftSoGP1fVaMZ5kt7HH5/aQ8BzEuRdo82rSrmSNfLKBGLfgu86f/B2m7HtPekZiwbUeYWtvotWQEgH0HXQu0ka/Wrq+XXoofnuGeEkzkiAQJoBB2VHPBUPxwzDg9VM8K7sRGFwJ9AFQFHk++bkI5yR38rRJH3ezfo2DoEsOYI2I9A3ADZgxvI3fCxcfl3EBvp29DUQyO4PkfXQJr7UjeiHfjE5GZAHa5aorJWuzFOOyKQutbI6TsyJ2pj7iPOix+CEy47f9sxnL/I4BsPhGSCGXp0u38+8OuzXin0+18Z93zcjwLX5t4Eeb+CFBid78477iuXNAVwSUQDDUKzxFd6sX7Al8KezSBhX34VCTC95wf8qCUP1WKj1roNzwaNqQ9xhHBVsuYfUleRt1kXk+Gyi/5+ays= root@kali
```

The other type of keys are the private keys that are used to connect to other machines. They are by default named `id_rsa`, hence we can search for these keys as well.

```bash
TCM@debian:~$ find / -name id_rsa 2> /dev/null
/backups/supersecretkeys/id_rsa
```

Once we find a key that we can access, the next step is to figure out which machine can be accessed with the discovered key.

