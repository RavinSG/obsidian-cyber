---
aliases:
  - RCE
---
This vulnerability exists because applications often *use functions in programming languages* such as PHP, Python and NodeJS to pass data to and to *make system calls on the machine’s operating system*. For example, taking input from a field and searching for an entry into a file. Take this code snippet below as an example:

![[RCE Vulnerability.png]]

In this code snippet, the application takes data that a user enters in an input field named `$title` to search a directory for a song title

An attacker could abuse this application by injecting their own commands for the application to execute. Rather than using `grep` to search for an entry in `songtitle.txt`, they could ask the application to read data from a more sensitive file.

Command Injection can be detected in mostly one of two ways:
- Blind command injection - This type of injection is where there is no direct output from the application when testing payloads. You will have to investigate the behaviours of the application to determine whether or not your payload was successful.

- Verbose command injection - This type of injection is where there is direct feedback from the application once you have tested a payload. For example, running the `whoami` command to see what user the application is running under. The web application will output the username on the page directly.
### Detecting Blind Command Injection

For this type of command injection, we will need to use payloads that will cause *some time delay*. For example, the `ping` and `sleep` commands are significant payloads to test with. 

Using `ping` as an example, the application will hang for *x seconds* in relation to how many pings you have specified.

Another method of detecting blind command injection is by forcing some output. This can be done by using redirection operators such as `>`.  For example, we can tell the web application to execute commands such as `whoami` and *redirect that to a file*. We can then use a command such as `cat` to read this newly created file’s contents.

The `curl` command is a great way to test for command injection. This is because you are able to use curl to *deliver data to and from an application* in your payload. Take this code snippet below as an example, a simple curl payload to an application is possible for command injection.

```bash
curl http://vulnerable.app/process.php%3Fsearch%3DThe%20Beatles%3B%20whoami
```

### Useful Payloads

| Payload  | Description                                                                                                                                                                                                          |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `whoami` | See what user the application is running under.                                                                                                                                                                      |
| `ls`     | List the contents of the current directory. You may be able to find files such as configuration files, environment files (tokens and application keys), and many more valuable things.                               |
| `ping`   | This command will invoke the application to hang. This will be useful in testing an application for blind command injection.                                                                                         |
| `sleep`  | This is another useful payload in testing an application for blind command injection, where the machine does not have ping installed.                                                                                |
| `nc`     | Netcat can be used to spawn a reverse shell onto the vulnerable application. You can use this foothold to navigate around the target machine for other services, files, or potential means of escalating privileges. |
