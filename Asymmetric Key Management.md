
- Choose an encryption system with an *algorithm in the public domain* that has been thoroughly vetted by industry experts. Be wary of systems that use a “black-box” approach and maintain that the secrecy of their algorithm is critical to the integrity of the cryptosystem.
  
- Use a *key length* that balances your security requirements with performance considerations. Also, ensure that your key is truly random, or, in cryptographic terms, that it has sufficient entropy. You should also understand the limitations of your cryptographic algorithm and avoid the use of any known weak keys.
  
- *Keep your private key secre*t! Do not, under any circumstances, allow anyone else to gain access to your private key. Remember, allowing someone access even once permanently compromises all communications that take place (past, present, or future) using that key and allows the third party to successfully impersonate you.
  
- *Retire keys* when they've served a useful life. Many organisations have mandatory key rotation requirements to protect against undetected key compromise. Continued reuse of a key creates more encrypted material that may be used in cryptographic attacks.
  
- *Back up your key*! If you lose the file containing your private key because of data corruption, disaster, or other circumstances, you'll certainly want to have a backup available. You may want to either create your own backup or use a key escrow service that maintains the backup for you. In either case, ensure that the backup is handled in a secure manner.
  
- Hardware security modules (HSMs) also provide an effective way to manage encryption keys. These hardware devices store and manage encryption keys in a secure manner that prevents humans from ever needing to work directly with the keys. Cloud providers, such as Amazon and Microsoft, also offer cloud-based HSMs that provide secure key management for IaaS services.