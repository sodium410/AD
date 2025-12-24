mitm6 attack is a technique that exploits the default behavior of Windows systems,  
which prioritize IPv6 over IPv4, even in environments that do not intentionally use IPv6.  

First thing – check if ipv6 is enabled ! And if any DNS configured !  
Netsh interface ipv6 show interface  
Ipconfig /all  

**How the mitm6 Attack Works in Detail**:  
Phase 1: Rogue IPv6 Configuration (DNS Takeover)  
When a Windows machine boots up or connects to a network, it automatically sends out DHCPv6 solicit packets  
to discover available IPv6 configuration, even if the network is primarily IPv4-based.  
The Action: The attacker runs the mitm6 tool. It listens for these requests and responds faster than legitimate servers.  
The Trick: mitm6 assigns the victim a link-local IPv6 address and—critically—sets the attacker’s machine as the default DNS server.  
The Result: The victim machine now trusts the attacker for all domain name resolution.  

Phase 2: WPAD Spoofing and Interception  
Windows uses a feature called Web Proxy Auto-Discovery (WPAD) to find network proxies.  
The Action: When the victim machine attempts to resolve the location of the WPAD file, it queries the newly assigned DNS server (the attacker).  
The Trick: mitm6 responds to queries for wpad by pointing the victim to the attacker's IP address. wpad.domain.com //is the address pattern by default   
The Result: The victim’s browser attempts to download the proxy configuration file from the attacker  

Phase 3: NTLM Relay (The Payoff)  
The attack is typically combined with a tool like ntlmrelayx to relay authentication attempts.  
The Action: When the victim requests the WPAD file, ntlmrelayx prompts for authentication, forcing the victim's Windows machine to send NTLM credentials.  
The Relay: ntlmrelayx intercepts these NTLM challenges/responses and relays them in real-time  
to other services (like LDAP or SMB) on the network, such as Domain Controllers  

Phase4: Privilege escalation & domain compromise:  
if relayed user is domain admin - new user account is auto created by ntlmrelay  
can abuse default AD configurations to create machine account and modify machine account to impersonate other users  

**Attack**:  
ntlmrelayx.py -6 –t ldaps://192.x.x.x -wh fakewpad.marvel.local -l lootme     //start this first    
check lootme folder for all collected data  //ip is that of AD ldap server //  
sudo mitm6 –d marvel.local    //next run this, serves ipv6 responses for that domain  

Don’t run for more than 10 mins and use on local domain as it causes problems !!  
trigger an event such as system reboot or somebody login !!  

Prevention: SMB/LDAP signing, disable ipv6 and dhcp6 traffic, disable wpad  

**Passback attacks**: printer ldap creds stored, change ip to kali - tcpdump   
