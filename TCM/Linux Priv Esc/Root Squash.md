
Root squashing is a security feature used in NFS (Network File System) to *prevent remote root users from having root access on shared filesystems*.

When a remote NFS client mounts a directory from a server, the client’s root user is mapped to a non-privileged user, typically `nobody` or a custom low-privileged UID.

We can check the `/ect/exports` file to see which directories are shared and with what permissions. If there is a share with the `no_root_squash` option present, we can exploit it to gain access.

```bash
TCM@debian:~$ cat /etc/exports 
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#

/tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)
#/tmp *(rw,sync,insecure,no_subtree_check)   
```

We see that the `/tmp` directory has `no_root_squash` set. Which means we can mount this `/tmp` folder to our machine and create files as `root`.