https://app.letsdefend[.]io/challenge/ics-fuelstation

# Threat Hunting Report - ICS Fuel Station

## Scenario
Demonstrate your **threat hunting** and **network traffic** analysis skills by uncovering a C2 communications–related incident. On one of the organization’s critical endpoints, a suspicious file with an unusual extension was flagged by security tooling. Analyze the provided network traffic to trace the attacker’s activity and identify the threat.
---

## Question 1: When did port scanning activity start?

### Answer: 2025–08–27 07:27:31
[YYYY-MM-DD HH:MM:SS]

### Analysis:
Identified the beginning of port scanning activity by analyzing network traffic patterns 
The following display filter was applied:

_tcp.flags.syn ==1 && tcp.flags.ack == 0_

### Evidence:
![alt text](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%201.png)

---

## Question 2: When did the port scanning activity stop?

### Answer: 2025–08–27 07:27:34
[YYYY-MM-DD HH:MM:SS]

### Analysis:
To remain consistent and avoid false conclusions, the same scan-detection logic was used as in the previous step. In Wireshark, the following display filter was applied:

_ip.src == 10.10.32.130 && tcp.flags.syn == 1 && tcp.flags.ack == 0_

### Evidence:
![Q2](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%202.png)

---

## Question 3: What is the IP address of the internal asset used by the attacker?
You can see this information in the previous screenshot under the “source” section
### Answer:10.10.32.130

---

## Question 4: Which port (other than the tank gauge system) was open?

### Answer:21

### Analysis:
To identify open ports, the following Wireshark display filter was applied:

_tcp.flags.syn == 1 && tcp.flags.ack == 1_

### Evidence:
![Q4](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%204.png)

---

## Question 5: Which port was used by the ICS system?

### Answer:10001

### Analysis:
Using the following display filter:

_tcp.flags.syn == 1 && tcp.flags.ack == 1_
Only two ports responded positively to the scan:

Port 21, which was identified as an FTP service
Another high-numbered port that consistently responded with SYN, ACK packets

### Evidence:
![Q5](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%204.png)

---

## Question 6: What is the vendor of the Automated Tank Gauge system?

### Answer:Veeder

### Analysis:
In Wireshark, the traffic was narrowed down to only packets communicating with the ICS service by applying the filter:

tcp.port == 10001

### Evidence:
![Q6](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%206.png)

---

## Question 7: What is the name of the petroleum pump being attacked?

### Answer:LD Refinery

### Evidence:
![Q7](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%206.png)

---

## Question 8: What function code was used for unauthorized data access?

### Answer:I20200

### Analysis:
Using this knowledge, the following display filter was applied in Wireshark:

_tcp.port == 10001 && tcp.len > 0_

### Evidence:
![Q8](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%208.png)

---

## Question 9: At what time was a leak detected?

### Answer:03:14 AM

### Analysis:
A display filter is applied to focus only on meaningful tank gauge data:

_tcp.port == 10001 && tcp.len > 0_

### Evidence:
![Q9](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%209.png)

---

## Question 10: What was the status of the product in the inventory?

### Answer: FILLING-IN-PROGRESS
![Q10](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/ANSWER%2010.png)

## Question 11:What was the email address associated with the attacker?

### Answer:legiongroup@protonmail.com
![11](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/images/answer%2011.png)



# 🔍 Investigation Summary

- Port scanning activity detected
- Internal system compromised
- ICS protocol exploited
- Unauthorized data access performed
- Operational impact observed (fuel leak)

---

# 🧠 MITRE ATT&CK Mapping

- T1046 - Network Service Scanning  
- T1078 - Valid Accounts  
- T1040 - Network Sniffing  
- T1496 - Resource Manipulation  

---

# 📊 Timeline of Attack

1. Port scanning initiated  
2. Internal system compromise  
3. ICS communication established  
4. Unauthorized commands executed  
5. Data accessed  
6. Physical impact (fuel leak)

---

# 🚨 Indicators of Compromise (IOC)

- Attacker IP:10.10.32.130
- Open ports:21
- ICS protocol activity:10001

---

# 🛡️ Recommendations

- Restrict ICS network access
- Disable unused ports
- Monitor ICS protocols
- Implement intrusion detection for OT networks
- Enable logging and alerting

---

# Conclusion

The attacker successfully scanned the network, gained access to an internal system, and interacted with the ICS environment. Unauthorized commands were executed, resulting in operational impact. Immediate remediation and improved monitoring are required.
