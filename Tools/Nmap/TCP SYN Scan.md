
Unprivileged users are limited to connect scan. However, the *default scan mode is SYN scan*, and it requires a privileged (root or sudoer) user to run it. SYN scan does not need to complete the TCP 3-way handshake; instead, **it tears down the connection once it receives a response** from the server.

![[TCP SYN Scan.png]]

Because we didn’t establish a TCP connection, this *decreases the chances of the scan being logged*. We can select this scan type by using the `-sS` option.