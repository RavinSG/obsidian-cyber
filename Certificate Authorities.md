---
aliases:
  - CAs
  - CA
---
Certificate authorities (CAs) are the glue that binds the public key infrastructure together. These neutral organisations *offer notarisation services* for digital certificates. To obtain a digital certificate from a reputable CA, you must prove your identity to the satisfaction of the CA. The following list includes some of the major CAs who provide widely accepted digital certificates:

- Symantec
- IdenTrust
- Amazon Web Services
- GlobalSign
- Comodo
- Certum
- GoDaddy
- DigiCert
- Secom
- Entrust
- Actalis
- Trustwave

Nothing is preventing any organization from simply setting up shop as a CA. However, the certificates issued by a CA are only as good as the trust placed in the CA that issued them.

If you configure your browser to trust a CA, it will automatically trust all of the digital
certificates issued by that CA. *Browser developers preconfigure browsers* to trust the major CAs to avoid placing this burden on users.

**Registration authorities** (RAs) assist CAs with the burden of verifying users' identities prior to issuing digital certificates. They do not directly issue certificates themselves, but they play an important role in the certification process, *allowing CAs to remotely validate user identities*.

Certificate authorities must carefully *protect their own private keys* to preserve their trust relationships. To do this, they often use an **offline CA** to protect their **root certificate**, the top-level certificate for their entire PKI.

This offline CA is disconnected from networks and powered down until it is needed. The offline CA *uses the root certificate to create subordinate intermediate CAs* that serve as the online CAs used to issue certificates on a routine basis.

In the CA trust model, the use of a series of intermediate CAs is known as **certificate chaining**. To validate a certificate, the browser verifies the identity of the intermediate CA(s) first and then traces the path of trust back to a known root CA, verifying the identity of each link in the chain of trust.

Certificate authorities do not need to be third-party service providers. Many organisations operate *internal CAs* that provide self-signed certificates for use inside an organization.

- [[Certificate Generation and Destruction]]
- [[Certificate Formats]]