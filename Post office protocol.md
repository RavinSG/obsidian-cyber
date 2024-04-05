---
aliases:
  - POP
tags:
  - Protocol
---
Post office protocol (**POP**) is an [[Application Layer]] protocol used to *manage and retrieve email from a mail server*. 

Many organisations have a dedicated mail server on the network that handles incoming and outgoing mail for users on the network. User devices will send requests to the remote mail server and download email messages locally. 

If you have ever refreshed your email application and had new emails populate in your inbox, you are experiencing POP and internet message access protocol ([[Internet Message Access Protocol|IMAP]]) in action. 

Unencrypted, plaintext authentication uses TCP/UDP port 110 and encrypted emails use Secure Sockets Layer/Transport Layer Security ([[Secure Sockets Layer|SSL]]/[[Transport Layer Security|TLS]]) over TCP/UDP port 995.  

When using POP, mail has to *finish downloading* on a local device *before it can be read* and it does not allow a user to sync emails.

