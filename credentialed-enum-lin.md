**Enumeration with ldapdomaindump**:  
sudo ldapdomaindump ldaps://10.1.1.1 -u 'v-root\soda' -p mypassword  
ls   //list all the collected data  

**Enumeration with bloodhound**:  bloodhound is both collector & gui, sharphound - collector for windows  
sudo pip install bloodhound  
sudo neo4j start  //starts db - http://localhost:7474  - neo4j : neo4j  
sudo bloodhound    //start the gui neo4j : neo4j  
mkdir marvel.local && cd marvel.local //create a folder to store scanned data  
sudo bloodhound-python -d marvel.local -u fcastle -p Password1 -ns 10.1.1.1 -c all  //collect all  
Either zip all json files or simply upload all files into bloodhound gui  

**Plumhound**: bloodhound for blue and purple teams,  have bloodhound runnin with data – as this pulls info from there !   
sudo git clone https://github.com/PlumHound/PlumHound.git   
cd into the folder and run sudo pip install –r requirements.txt  
sudo python3 PlumHound.py --easy –p neo4j  //just test- that's a password for bloodhound console!  
sudo python3 PlumHound.py -x tasks/default.tasks -p neo4j   //run default tasks  
cd reports And open index.html -- good !  

**Pingcastle**:  windows based - can be run as standard user - run as admin in pnpt why ?    
https://www.pingcastle.com/download  
run pingcastle.exe   // can also be run remotely  takes 3-5 mins  
