
NIST provide a set of recommendations that are broadly adopted as part of NIST SP 800-63B as part of their Digital Identity Guidelines. 

Their best practices include using Show Password to prevent typos, using password managers, ensuring secrets are stored securely using salting and secure hashing methods, locking accounts after multiple attempts, and employing multi-factor authentication.

In addition to those broad recommendations, NIST specifically suggests that modern password practices follow a few guidelines:

- Reducing password complexity requirements and instead emphasising length
- Not requiring special characters
- Allowing ASCII and Unicode characters in passwords
- Allowing pasting into password fields to allow password managers to work properly
- Monitoring new passwords to ensure that easily compromised passwords are not used
- Eliminating password hints

>[!warning] 
> One of the most common password length setting requirements for many organisations was driven by the **LAN Manager (LM) hash**. This short hash was generated if passwords had less than 15 characters in them, resulting in many organisations setting their password requirement to more than 15 characters to prevent insecure hash storage.

### Password Managers

Password managers are designed to help users create secure passwords, then store and manage them along with related data like notes, URLs, or other important information associated along with the secrets they're used to store.

Use of password managers for both individual and organisations is common, and using a password manager is a common recommendation due to reduction in password reuse and the greater likelihood of using complex and long passwords.

### Passwordless

This includes authentication methods that rely on something you have--security tokens, one time password applications, or certificates--or something you are, like biometric factors.

For individuals, one option that provides a high level of security is a *security key*. These are hardware devices that support things like one-time passwords, public key cryptography for security certificates, and various security protocols like **[[Fast Identity Online|FIDO]]** and Universal 2nd Factor (**U2F**). 

Passwordless authentication is intended to reduce the friction and risks that are associated with passwords.

>[!info]
>**FIDO2** is an open authentication standard that supports both the **W3C** Web Authentication specification and the Client to Authenticator Protocol (**CTAP**), which is used to communicate between browsers, operating systems, and similar clients and the FIDO2 device.
>
>FIDO2 authentication relies on key pairs, with a public key sent to services and private keys that remain on the device.

