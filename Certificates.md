
Digital certificates provide communicating parties with the **assurance** that the people they are communicating with truly are who they claim to be. Digital certificates are essentially *endorsed copies of an individual's public key*. When users verify that a certificate was signed by a trusted certificate authority ([[Certificate Authorities|CA]]), they know that the public key is legitimate.

Digital certificates contain specific identifying information, and their construction is *governed by an international standard—X.509*. Certificates that conform to X.509 contain the following certificate attributes:

- Version of X.509 to which the certificate conforms
- Serial number (from the certificate creator)
- Signature algorithm identifier (specifies the technique used by the certificate authority to digitally sign the contents of the certificate)
- Issuer name (identification of the certificate authority that issued the certificate)
- Validity period (specifies the dates and times—a starting date and time and an expiration date and time—during which the certificate is valid)
- Subject's Common Name (CN) that clearly describes the certificate owner (e.g., “certmike.com”)
- Certificates may optionally contain Subject Alternative Names (SAN) that allow you to specify additional items (IP addresses, domain names, and so on) to be protected by the single certificate.
- Subject's public key (the meat of the certificate—the actual public key the certificate owner used to set up secure communications)

The current version of X.509 (version 3) supports certificate extensions—customised variables containing data inserted into the certificate by the certificate authority to support tracking of
certificates or various applications.

Certificates may be issued for a variety of purposes. These include providing assurance for the public keys of 

- Computers/machines
- Individual users
- Email addresses
- Developers (code-signing certificates)

The subject of a certificate may include a *wildcard* in the certificate name, indicating that the certificate is good for subdomains as well. The wildcard is designated by an asterisk character. For example, a wildcard certificate issued to `*.certmike.com` would be valid for all of the following domains:

- `certmike.com`
- `www.certmike.com`
- `mail.certmike.com`
- `secure.certmike.com`

>[!info] Wildcard Certificates
>Wildcard certificates are only good for one level of subdomain. Therefore, the
`*.certmike.com` certificate **would not be valid** for the `www.cissp.certmike.com` subdomain.


