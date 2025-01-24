
One easy way to check whether there are leaked passwords is to check the user's **history**!

We can search for passwords in plain text files by executing a system-wide `grep` command as follow.

```bash
grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null
```

This will also colour code the text in red for easy identification.