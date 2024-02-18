---
aliases:
  - IMAP
tags:
  - Protocol
---
**IMAP** is used for incoming email. It *downloads the headers of emails*, but not the content. The content remains on the email server, which allows users to access their email from multiple devices. 

IMAP uses TCP port 143 for unencrypted email and TCP port 993 over the TLS protocol. 

Using IMAP allows users to partially read email before it is finished downloading and to sync emails. However, *IMAP is slower than POP3*.