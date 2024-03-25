---
URL: https://tryhackme.com/room/passiverecon
---
In passive reconnaissance, we **rely on publicly available knowledge**. It is the knowledge that we can access from publicly available resources without directly engaging with the target. Think of it like we are looking at target territory from afar without stepping foot on that territory.

Passive reconnaissance activities include many activities, for instance:

- Looking up DNS records of a domain from a public DNS server.
- Checking job ads related to the target website.
- Reading news articles about the target company.

[[Active Reconnaissance]], on the other hand, cannot be achieved so discreetly. It requires **direct engagement with the target**. Think of it like we check the locks on the doors and windows, among other potential entry points.

Examples of active reconnaissance activities include:

- Connecting to one of the company servers such as HTTP, FTP, and SMTP.
- Calling the company in an attempt to get information (social engineering).
- Entering company premises pretending to be a repairman.

## whois

WHOIS is a request and response protocol that follows the [RFC 3912](https://www.ietf.org/rfc/rfc3912.txt) specification. A WHOIS server listens on **TCP port 43** for incoming requests. The domain registrar is responsible for maintaining the WHOIS records for the domain names it is leasing. 

The WHOIS server replies with various information related to the domain requested. Of particular interest, we can learn:

- *Registrar*: Via which registrar was the domain name registered?
- *Contact info of registrant*: Name, organization, address, phone, among other things. (unless made hidden via a privacy service)
- *Creation, update, and expiration dates*: When was the domain name first registered? When was it last updated? And when does it need to be renewed?
- *Name Server*: Which server to ask to resolve the domain name?

When we run `whois` on `tryhackme.com`, we get the following:

```bash
user@TryHackMe$ whois tryhackme.com
[Querying whois.verisign-grs.com]
[Redirected to whois.namecheap.com]
[Querying whois.namecheap.com]
[whois.namecheap.com]
Domain name: tryhackme.com
Registry Domain ID: 2282723194_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.namecheap.com
Registrar URL: http://www.namecheap.com
Updated Date: 2021-05-01T19:43:23.31Z
Creation Date: 2018-07-05T19:46:15.00Z
Registrar Registration Expiration Date: 2027-07-05T19:46:15.00Z
Registrar: NAMECHEAP INC
Registrar IANA ID: 1068
Registrar Abuse Contact Email: abuse@namecheap.com
Registrar Abuse Contact Phone: +1.6613102107
Reseller: NAMECHEAP INC
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registry Registrant ID: 
Registrant Name: Withheld for Privacy Purposes
Registrant Organization: Privacy service provided by Withheld for Privacy ehf
[...]
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2021-08-25T14:58:29.57Z <<<
For more information on Whois status codes, please visit https://icann.org/epp
```

First, we notice that we were redirected to `whois.namecheap.com` to get our information. In this case and at the time being, namecheap.com is maintaining the WHOIS record for this domain name. Furthermore, we can see the creation date along with the last-update date and expiration date.

Next, we obtain information about the registrar and the registrant. We can find the registrant’s name and contact information unless they are using some privacy service. Although not displayed above, we get the admin and tech contacts for this domain. Finally, we see the domain name servers that we should query if we have any DNS records to look up.

The information collected can be inspected to *find new attack surfaces*, such as social engineering or technical attacks. For instance, depending on the scope of the penetration test, we might consider an *attack against the email server of the admin user or the DNS servers*, assuming they are owned by our client and fall within the scope of the penetration test.

It is important to note that due to automated tools abusing WHOIS queries to harvest email addresses, many WHOIS services take measures against this. They might *redact email addresses*, for instance. Moreover, many registrants subscribe to privacy services to avoid their email addresses being harvested by spammers and keep their information private.

## nslookup

We can use `nslookup` to find the IP addresses of domain names. `nslookup` stands for name server lookup. 

We need to issue the command `nslookup DOMAIN_NAME`, for example, `nslookup tryhackme.com`. Or, more generally, we can use `nslookup OPTIONS DOMAIN_NAME SERVER`. These three main parameters are:

- `OPTIONS` contains the **query type**. For instance, we can use *A* for *IPv4* addresses and *AAAA* for *IPv6* addresses.
- `DOMAIN_NAME` is the domain name we are looking up.
- `SERVER` is the *DNS server that we want to query*. We can choose any local or public DNS server to query. Cloudflare offers `1.1.1.1` and `1.0.0.1`, Google offers `8.8.8.8` and `8.8.4.4`, and Quad9 offers `9.9.9.9` and `149.112.112.112`. There are many more public DNS servers that we can choose from if we want alternatives to our ISP’s DNS servers.

For instance, `nslookup -type=A tryhackme.com 1.1.1.1`  can be used to return all the IPv4 addresses used by tryhackme.com.

```Shell
user@TryHackMe$ nslookup -type=A tryhackme.com 1.1.1.1
Server:		1.1.1.1
Address:	1.1.1.1#53

Non-authoritative answer:
Name:	tryhackme.com
Address: 172.67.69.208
Name:	tryhackme.com
Address: 104.26.11.229
Name:	tryhackme.com
Address: 104.26.10.229
```

This lookup is helpful to know from a penetration testing perspective. In the example above, we started with one domain name, and we obtained three IPv4 addresses. *Each of these IP addresses can be further checked for insecurities*, assuming they lie within the scope of the penetration test.

Let’s say we want to learn about the email servers and configurations for a particular domain. We can issue `nslookup -type=MX tryhackme.com`. Here is an example:

```bash
user@TryHackMe$ nslookup -type=MX tryhackme.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
tryhackme.com	mail exchanger = 5 alt1.aspmx.l.google.com.
tryhackme.com	mail exchanger = 1 aspmx.l.google.com.
tryhackme.com	mail exchanger = 10 alt4.aspmx.l.google.com.
tryhackme.com	mail exchanger = 10 alt3.aspmx.l.google.com.
tryhackme.com	mail exchanger = 5 alt2.aspmx.l.google.com.
```

We can see that `tryhackme.com`’s current email configuration uses Google. We notice that `aspmx.l.google.com` is the *default* Mail Exchange server, which *has order 1*. 

*If it is busy or unavailable*, the mail server will attempt to connect to the *next in order* mail exchange servers, `alt1.aspmx.l.google.com` or `alt2.aspmx.l.google.com`

## dig

For more advanced DNS queries and additional functionality, we can use **dig**, the acronym for “**Domain Information Groper**,”

We can use dig `DOMAIN_NAME`, but to specify the record type, we would use `dig DOMAIN_NAME TYPE`. Optionally, we can select the server we want to query using `dig @SERVER DOMAIN_NAME TYPE`.

```bash
$ dig tryhackme.com MX 

; <<>> DiG 9.19.19-1-Debian <<>> tryhackme.com MX
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 56329
;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; MBZ: 0x0005, udp: 4096
;; QUESTION SECTION:
;tryhackme.com.			IN	MX

;; ANSWER SECTION:
tryhackme.com.		5	IN	MX	1 aspmx.l.google.com.
tryhackme.com.		5	IN	MX	10 alt3.aspmx.l.google.com.
tryhackme.com.		5	IN	MX	10 alt4.aspmx.l.google.com.
tryhackme.com.		5	IN	MX	5 alt1.aspmx.l.google.com.
tryhackme.com.		5	IN	MX	5 alt2.aspmx.l.google.com.

;; Query time: 52 msec
;; SERVER: 192.168.139.2#53(192.168.139.2) (UDP)
;; WHEN: Mon Mar 25 11:06:31 AEDT 2024
;; MSG SIZE  rcvd: 157
```

A quick comparison between the output of nslookup and dig shows that dig returned more information, such as the *TTL* (Time To Live) by default.

## [[DNSDumpster]]

## [[Shodan.io]]


### War Driving

One common goal of penetration testers is to identify wireless networks that may present a means of gaining access to an internal network of the target without gaining physical access to the facility. Testers use a technique called *war driving*, where they *drive by facilities in a car equipped with high-end antennas* and attempt to eavesdrop on or connect to wireless networks. Recently, testers have expanded this approach to the use of drones and unmanned aerial vehicles (UAVs) in a technique known as *war flying*.