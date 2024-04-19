
A common implementation of a *second factor* is the use of one-time passwords. One-time passwords are an important way to combat password theft and other password-based attacks.

An attacker can obtain a one-time password but *cannot continue using it*, and it means that brute-force attacks will be constantly attempting to identify a constantly changing target.

There are two primary models for generation of one-time passwords.

- *Time-based one-time password* (**TOTP**), which use an algorithm to derive a one-time password using the current time as part of the code-generation process. Authentication applications like Google Authenticator use TOTP. The *code is valid for a set period of time* and then moves on to the next time-based code, meaning that even if a code is compromised it will be valid for only a relatively short period of time.![[TOTP.png]]
  
- *HMAC-based onetime password* (**HOTP**), HMAC stands for hash-based message authentication codes. HOTP uses a *seed value* that both the token or HOTP code-generation application and the validation server use, as well as a *moving factor*. For HOTP tokens that work when you press a button, the moving factor is a counter, which is also stored on the token and the server. Since the codes are iterative, they can be *checked from the last known use* of the token, with iterations forward until the current press is found.![[HOTP.png]]


>[!danger] Attacking One-Time Passwords
>TOTP passwords can be stolen by either *tricking a user into providing them*, gaining access to a device like a phone where they are generated, or otherwise having near real-time access to them. This means that attackers must use a stolen TOTP password immediately. 
>
>One-time passwords sent via *SMS can be redirected* using a cloned SIM, or if the phone is part of a VoIP network, by compromising the VoIP system or account and redirecting the SMS factor.
>
>One of the most successful attacks is overwhelming end users with repeated validation requests so that they enter an OTP value to make the request stop!

Although one-time passwords that are dynamically generated as they are needed are more common, at times there is a need for a one-time password that *does not require a device or connectivity*. In those cases, **static codes** remain a useful option. Static codes are also algorithmically generated like other one-time passwords but are pre-generated and often printed or stored in a secure location.

This creates a new risk model, which is that the paper they are printed on could be stolen, or if they are stored electronically the file they're stored in could be lost or accessed.