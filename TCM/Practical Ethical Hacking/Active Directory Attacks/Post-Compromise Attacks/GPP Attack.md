
Group Policy Preference (**GPP**) allowed admins to create policies using embedded credentials. These credentials were encrypted and stored under a "cpassword" XML attribute in the *SYSVOL* directory in AD.

However, the **encryption key was hardcoded** to the OS by Microsoft and it was **accidentally leaked**. Which means anyone can now easily decrypt any of the encrypted credentials using the leaked key.

This was patched in 2014 in the *MS14-025* update, but it **does not work retrospectively**. Hence, passwords created before the patch are vulnerable and can still exist in organisational environments.

There is a metasploit module named `auxiliary/scanner/smb/smb_enum_gpp` which can enumerate files in a domain controller to identify files with the "cpassword" attribute and decrypt them.

In addition there is a built-in tool in Kali named `gpp-decrypt` which can be used to decrypt the passwords manually.