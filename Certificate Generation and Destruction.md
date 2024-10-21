
### Enrolment

When you want to obtain a digital certificate, you must *first prove your identity* to the CA in some manner; this process is called enrolment.

Once you've satisfied the certificate authority regarding your identity, you provide them with your public key in the form of a **Certificate Signing Request** (CSR). The CA next creates an X.509 digital certificate containing your identifying information and a copy
of your public key.

The CA then digitally signs the certificate using the CA's private key and provides you with a copy of your signed digital certificate.

Certificate authorities issue different types of certificates depending upon the level of identity verification that they perform. 

The simplest, and most common, certificates are *Domain Validation* (DV) certificates, where the CA simply verifies that the certificate subject has control of the domain name. 

*Extended Validation* (EV) certificates provide a higher level of assurance and the CA takes steps to verify that the certificate owner is a legitimate business before issuing the certificate.

### Verification

When you receive a digital certificate from someone with whom you want to communicate, you verify the certificate by **checking the CA's digital signature using the CA's public key**. 

Next, you must check and ensure that the certificate was not revoked using a *certificate revocation list* (CRL) or the *Online Certificate Status Protocol* (OCSP).

At this point, you may assume that the public key listed in the certificate is authentic, provided that it satisfies the following requirements:

- The digital signature of the CA is authentic
- You trust the CA
- The certificate is not listed on a CRL
- The certificate actually contains the data you are trusting

When purchasing a certificate, you choose a CA that is widely trusted. If a CA is not included in, or is later pulled from, the list of CAs trusted by a major browser, it will greatly limit the usefulness of your certificate.

**Certificate pinning** approaches instruct browsers to attach a certificate to a subject for an extended period of time. When sites use certificate pinning, the *browser associates that site with their public key*. This allows users or administrators to notice and intervene if a certificate unexpectedly changes.

### Revocation

Occasionally, a certificate authority needs to revoke a certificate. This might occur for one of the following reasons:

- The certificate was compromised (for example, the certificate owner accidentally gave away the private key).
- The certificate was erroneously issued (for example, the CA mistakenly issued a certificate without proper verification).
- The details of the certificate changed (for example, the subject's name changed).
- The security association changed (for example, the subject is no longer employed by the organisation sponsoring the certificate).

You can use three techniques to verify the authenticity of certificates and identify revoked certificates:

- **Certificate Revocation Lists** (CRLs) are maintained by the various certificate authorities and contain the serial numbers of certificates that have been issued by a CA and have been revoked along with the date and time the revocation went into effect. The major disadvantage to certificate revocation lists is that they must be downloaded and cross-referenced periodically, introducing a period of latency between the time a certificate is revoked and the time end users are notified of the revocation.

- **Online Certificate Status Protocol** (OCSP) This protocol eliminates the latency inherent in the use of certificate revocation lists by providing a means for real-time certificate verification. When a client receives a certificate, it sends an OCSP request to the CA's OCSP server. The server then responds with a status of valid, invalid, or unknown. The browser uses this information to determine whether the certificate is valid. 
  
- **Certificate Stapling** The primary issue with OCSP is that it places a significant burden on the OCSP servers operated by certificate authorities. These servers must process requests from every single visitor to a website or other user of a digital certificate, verifying that the certificate is valid and not revoked.

Certificate stapling is an extension to the Online Certificate Status Protocol that relieves some of the burden placed upon certificate authorities by the original protocol.

In certificate stapling, the *web server contacts the OCSP* server itself and receives a signed and timestamped response from the OCSP server, which it then *attaches*, or staples, *to the digital certificate*.

Then, when a user requests a secure web connection, the web server sends the certificate with the stapled OCSP response to the user. The *user's browser* then *verifies that the certificate is authentic* and also validates that the stapled OCSP response is genuine and recent.

The time savings come when the *next user visits* the website. The web server can simply *reuse the stapled certificate*, without recontacting the OCSP server.