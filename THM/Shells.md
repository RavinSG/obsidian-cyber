
At a high level, we are interested in two kinds of shell when it comes to exploiting a target: Reverse shells, and bind shells.

- **Reverse shells** are when the target is forced to execute code that *connects back to your computer*. On your own computer you would use one of the tools mentioned below to set up a listener which would be used to receive the connection. Reverse shells are a *good way to bypass firewall rules* that may prevent you from connecting to arbitrary ports on the target; however, the drawback is that, when receiving a shell from a machine across the internet, you would need to configure your own network to accept the shell.
	- [rev shells](https://www.revshells.com)
	- [pentestmonkey](https://web.archive.org/web/20200901140719/http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
	- [SecLists](https://github.com/danielmiessler/SecLists)
  
- **Bind shells** are when the code executed on the target is used to start a listener attached to a shell directly on the target. This would then be opened up to the internet, meaning you can connect to the port that the code has opened and obtain remote code execution that way. This has the advantage of not requiring any configuration on your own network, but may be prevented by firewalls protecting the target.
  
### Interactivity

Shells can be either interactive or non-interactive.

**Interactive**: If you've used Powershell, Bash, Zsh, sh, or any other standard CLI environment then you will be used to interactive shells. These allow you to interact with programs after executing them. For example, take the SSH login prompt:

![[Interactive Shell.png]]

Here you can see that it's asking interactively that the user type either yes or no in order to continue the connection. This is an interactive program, which requires an interactive shell in order to run.

**Non-Interactive** shells don't give you that luxury. In a non-interactive shell you are limited to using programs which *do not require user interaction* in order to run properly. Unfortunately, the majority of simple reverse and bind shells are non-interactive, which can make further exploitation trickier. Let's see what happens when we try to run SSH in a non-interactive shell:

![[Non-Interactive Shell.png]]

Notice that the `whoami` command (which is non-interactive) executes perfectly, but the `ssh` command (which is interactive) gives us no output at all.

### Shell Stabilisation

These shells are very unstable by default. Pressing `Ctrl + C` kills the whole thing. They are *non-interactive*, and often have strange formatting errors. This is due to netcat "shells" really being processes running inside a terminal, rather than being bonafide terminals in their own right. Fortunately, there are many ways to stabilise netcat shells on Linux systems. 
#### Python

This technique is applicable only to Linux boxes, as they will nearly always have Python installed by default. This is a three stage process:

1. The first thing to do is use `python -c 'import pty;pty.spawn("/bin/bash")'`, which uses Python to *spawn a better featured bash shell*; note that some targets may need the version of Python specified. If this is the case, replace python with python2 or python3 as required. At this point our shell will look a bit prettier, but we still won't be able to use tab autocomplete or the arrow keys, and Ctrl + C will still kill the shell.
   
2. Step two is: `export TERM=xterm` -- this will give us *access to term commands* such as `clear`.
   
3. Finally (and most importantly) we will background the shell using `Ctrl + Z`. Back in our own terminal we use `stty raw -echo; fg`. This does two things: first, it *turns off our own terminal echo* (which gives us access to tab autocompletes, the arrow keys, and Ctrl + C to kill processes). It then *foregrounds the shell*, thus completing the process.

![[Shell Stabilisation Python.png]]


```bash
python -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
#Ctrl + Z
stty raw -echo; fg
```
#### rlwrap

*rlwrap* is a program which, in simple terms, gives us access to history, tab autocompletion and the arrow keys immediately upon receiving a shell; however, some manual stabilisation must still be utilised if you want to be able to use `Ctrl + C` inside the shell. rlwrap is *not installed by default* on Kali, so first install it with sudo apt install rlwrap.

To use rlwrap, we invoke a slightly different listener:

```bash
rlwrap nc -lvnp <port>
```

Prepending our netcat listener with "rlwrap" gives us a much more fully featured shell. This technique is **particularly useful when dealing with Windows shells**, which are otherwise notoriously difficult to stabilise. When dealing with a Linux target, it's possible to completely stabilise, by using the same trick as in step three of the previous technique: background the shell with `Ctrl + Z`, then use `stty raw -echo; fg` to stabilise and re-enter the shell.

#### Socat

The third easy way to stabilise a shell is quite simply to use an initial *netcat shell as a stepping stone* into a more fully-featured socat shell. Bear in mind that this technique is **limited to Linux targets**, as a Socat shell on Windows will be no more stable than a netcat shell. To accomplish this method of stabilisation we would first transfer a [socat static compiled binary](https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/socat?raw=true) (a version of the program compiled to have no dependencies) up to the target machine. 


### Terminal Size

With any of the above techniques, it's useful to be able to change your terminal tty size. This is something that your terminal will do automatically when using a regular shell; however, it must be done manually in a reverse or bind shell if you want to use something like a text editor which overwrites everything on the screen.

First, open another terminal and run `stty -a`. This will give you a large stream of output. Note down the values for "rows" and columns:

![[Terminal tty size.png]]

Next, in your reverse/bind shell, type in:

`stty rows <number>` and `stty cols <number>`

Filling in the numbers you got from running the command in your own terminal. This will change the registered width and height of the terminal, thus allowing programs such as text editors which rely on such information being accurate to correctly open.