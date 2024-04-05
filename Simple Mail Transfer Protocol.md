---
aliases:
  - SMTP
tags:
  - Protocol
---
Simple Mail Transfer Protocol (**SMTP**) is used to transmit and route email from the sender to the recipient’s address. 

SMTP works with *Message Transfer Agent* (MTA) software, which searches DNS servers to resolve email addresses to IP addresses, to ensure emails reach their intended destination. 

SMTP uses TCP/UDP port 25 for unencrypted emails and TCP/UDP port 587 using TLS for encrypted emails. The TCP port 25 is often used by high-volume spam. SMTP helps to filter out spam by regulating how many emails a source can send at a time.

Email delivery over the Internet requires the following components:

- Mail Submission Agent (**MSA**)
- Mail Transfer Agent (**MTA**)
- Mail Delivery Agent (**MDA**)
- Mail User Agent (**MUA**)

![[Email delivery.png]]

The figure shows the following five steps that an email needs to go through to reach the recipient’s inbox:

1) A Mail User Agent (**MUA**), or simply an email client, has an email message to be sent. The MUA connects to a Mail Submission Agent (**MSA**) to send its message.
2) The MSA receives the message, checks for any errors before transferring it to the Mail Transfer Agent (**MTA**) server, commonly hosted on the same server.
3) The MTA will send the email message to the MTA of the recipient. The MTA can also function as a Mail Submission Agent (**MSA**).
4) A typical setup would have the MTA server also functioning as a Mail Delivery Agent (**MDA**).
5) The recipient will collect its email from the MDA using their email client.

Simple Mail Transfer Protocol (**SMTP**) is used to *communicate with an MTA* server. Because SMTP uses cleartext, where all commands are sent without encryption, we can use a basic *Telnet* client to connect to an SMTP server and *act as an email client (MUA)* sending a message.

