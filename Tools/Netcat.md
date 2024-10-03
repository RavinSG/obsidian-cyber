
Netcat or simply `nc` has different applications that can be of great value to a pentester. Netcat supports both TCP and UDP protocols. It can function *as a client* that connects to a listening port; alternatively, it can act *as a server* that listens on a port of your choice. Hence, it is a convenient tool that you can use as a simple client or server over TCP or UDP.

We can connect to a server, as same as Telnet, to collect its banner using `nc MACHINE_IP PORT`, Note that you might need to press `SHIFT+ENTER` after the GET line.

```bash
pentester@TryHackMe$ nc MACHINE_IP 80
GET / HTTP/1.1
host: netcat

HTTP/1.1 200 OK
Server: nginx/1.6.2
Date: Tue, 17 Aug 2021 11:39:49 GMT
Content-Type: text/html
Content-Length: 867
Last-Modified: Tue, 17 Aug 2021 11:12:16 GMT
Connection: keep-alive
ETag: "611b9990-363"
Accept-Ranges: bytes
...
```


We can also use netcat to *listen on a TCP port* and connect to a listening port on another system. For example, **SSH** is programmed to handle connections over port 22 to send all data and keys. We can connect to TCP port 22 with netcat:

```bash
RavinSG@htb[/htb]$ netcat 10.10.10.10 22

SSH-2.0-OpenSSH_8.4p1 Debian-3
```

As we can see, port 22 sent us its banner, stating that SSH is running on it. This technique is called *Banner Grabbing*, and can help identify what service is running on a particular port.

On the server system, where you want to open a port and listen on it, you can issue `nc -lp 1234 `or better yet, `nc -vnlp 1234`, which is equivalent to `nc -v -l -n -p 1234`, as you would remember from the Linux room. The exact order of the letters does not matter as long as the port number is preceded directly by `-p`.

| Option | Meaning                                                    |
| ------ | ---------------------------------------------------------- |
| `-l`   | Listen mode                                                |
| `-p`   | Specify the Port number                                    |
| `-n`   | Numeric only; no resolution of hostnames via DNS           |
| `-v`   | Verbose output (optional, yet useful to discover any bugs) |
| `-vv`  | Very Verbose (optional)                                    |
| `-k`   | Keep listening after client disconnects                    |

