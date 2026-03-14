source: https://www.malware-traffic-analysis.net/2026/01/31/index[.]html


BACKGROUND
As an analyst at a Security Operations Center (SOC), you check alerts for the past week and find a signature hit for ET MALWARE Lumma Stealer Victim Fingerprinting Activity that triggered on traffic from 153.92.1[.]49 over TCP port 80. The alert triggered on 2026-01-27 at 23:05 UTC.

Using the information, you retrieve a packet capture (pcap) of the traffic from the internal IP address that triggered the alert. Based on the pcap, you write up an incident report, so the incident responders can track down the computer and associated user.

The characteristics of your environment are:

LAN segment range:  10.1.21[.]0/24   (10.1.21[.]0 through 10.1.21[.]255)
Domain:  win11office[.]com
AD environment name:  WIN11OFFICE
Active Directory (AD) domain controller:  10.1.21[.]2 - WIN-LU4L24X3UB7
LAN segment gateway:  10.1.21[.]1
LAN segment broadcast address:  10.1.21[.]255


MY TASK
For this exercise, answer the following questions for your incident report:

1 What is the IP address of the infected Windows client?
2 What is the MAC address of the infected Windows client?
3 What is the host name of the infected Windows client?
4 What is the user account name from the infected Windows client?
5 What is the full name of the user from the user account?
6 What is the domain from 153.92.1[.]49 that triggered the alert for Lumma Stealer?


Task 1

According to the exercise description on Malware‑Traffic‑Analysis.net, the internal network uses the 10.1.21.0/24 subnet. The known infrastructure hosts are:

10.1.21.1 – Gateway

10.1.21.2 – Domain Controller

Therefore, the infected workstation must be another address within the same subnet.

The alert mentioned suspicious communication with the external IP address 153.92.1.49 over TCP port 80. To locate the internal host communicating with this address, a display filter was applied in Wireshark:

ip.addr == 153.92.1.49

This filter revealed HTTP traffic between the external server and an internal host. Inspection of the packets showed that the internal source address initiating the communication was:

10.1.21.58

Since this host is the internal system generating the suspicious outbound traffic to the flagged external IP address, it was identified as the infected Windows client.

Answer: 10.1.21.58


