Platform: Blue Team Labs Online (BTLO)
Scenario: Can you analyze logs from an attempted RDP bruteforce attack? One of our system administrators identified a large number of Audit Failure events in the Windows Security Event log. There are a number of different ways to approach the analysis of these logs! Consider the suggested tools, but there are many others out there!

Category: Incident Response

Type: Challenge

Difficulty: Medium

Files: BTLO_Bruteforce_Challenge.csv BTLO_Bruteforce_Challenge.evtx BTLO_Bruteforce_Challenge.txt

# Questions and Answers:

**1. How many Audit Failure events are there? (Format: Count of Events)**

-Open BTLO_Bruteforce_Challenge.csv. I use VS Code because it is much easier for me to use than Excel.
![image](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/Task%201%2C2%2C3.png)

Answer: 3103

I can see the answers to the second and third questions straight away, but let’s double-check them just in case


**2. What is the username of the local account that is being targeted? (Format: Username)**
![alt text](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/task%202.png)

answer: administrator


**3. What is the failure reason related to the Audit Failure logs? (Format: String)**
![alt text](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/Task%201%2C2%2C3.png)

Answer: Unknown user name or bad password


**4. What is the Windows Event ID associated with these logon failures? (Format: ID)**

Answer: 4625


**5. What is the source IP conducting this attack? (Format: X.X.X.X)**
-Using Event Viewer, go to Details tab and expand the EventData section.
![alt text](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/task%205.png)

Answer: 113.161.192.227


**6. What country is this IP address associated with? (Format: Country)**
Use VirusTotal to check the location of our IP address.
![alt text](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/task%206.png)

Answer: Vietnam


**7What is the range of source ports that were used by the attacker to make these login requests? (LowestPort-HighestPort — Ex: 100–541)**

Answer 49162–65534
