---
aliases:
  - TLS
  - SSL/TLS
---
Because of the close relation between SSL and TLS, one might be used instead of the other. However, TLS is more secure than [[Secure Sockets Layer|SSL]], and it has *practically replaced SSL*. 

An existing cleartext protocol can be upgraded to use encryption via SSL/TLS. We can *use TLS to upgrade HTTP, FTP, SMTP, POP3, and IMAP*, to name a few. 

Considering the case of **HTTP**. Initially, to retrieve a web page over HTTP, the web browser would need at least perform the following two steps:

- Establish a TCP connection with the remote web server
- Send HTTP requests to the web server, such as GET and POST requests.

*HTTPS requires an additional step* to encrypt the traffic. The new step takes place *after establishing a TCP* connection and *before sending HTTP* requests. Consequently, HTTPS requires at least the following three steps:

- Establish a TCP connection
- Establish SSL/TLS connection
- Send HTTP requests to the webserver


To establish an SSL/TLS connection, the client needs to perform the proper handshake with the server. Based on [RFC 6101](https://datatracker.ietf.org/doc/html/rfc6101), the SSL connection establishment will look like the figure below.

![[SSL Handshake.png]]

After establishing a TCP connection with the server, the client establishes an SSL/TLS connection. 

1) The client sends a **ClientHello** to the server to *indicate its capabilities*, such as supported algorithms.
2) The server responds with a **ServerHello**, indicating the *selected connection parameters*. The server provides its **certificate** if server authentication is required. Moreover, it might send additional information necessary to generate the master key, in its **ServerKeyExchange** message, before sending the **ServerHelloDone** message to indicate that it is done with the negotiation.
3) The *client* responds with a **ClientKeyExchange**, which contains additional information required to generate the master key. Furthermore, it *switches to use encryption* and informs the server using the **ChangeCipherSpec** message.
4) The *server switches to use encryption* as well and informs the client in the **ChangeCipherSpec** message.

Consequently, once an SSL/TLS handshake has been established, HTTP requests and exchanged data won’t be accessible to anyone watching the communication channel.


