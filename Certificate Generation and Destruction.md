
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
