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

Answer: 10.1.21.58 ![alt text](https://github.com/Anton-Bielikov/cybersecurity-portfolio/blob/main/network-forensics/pcap-investigation-1/task%201.png)

Task 2
After identifying the infected workstation as 10.1.21.58, the next step was to determine its MAC address using the provided PCAP file in Wireshark.

First, a display filter was applied to isolate all traffic related to the infected host:

ip.addr == 10.1.21.58

This filter allowed inspection of packets sent or received by the suspected system.

To identify the MAC address associated with this IP, ARP traffic was analyzed. ARP packets reveal the relationship between IP addresses and hardware (MAC) addresses on the local network. By reviewing ARP requests and responses involving 10.1.21.58, the corresponding Ethernet address was observed.

Opening one of these packets and expanding the Ethernet II header shows the Source MAC address of the host generating the traffic.

The analysis revealed the following mapping:

IP address: 10.1.21.58

MAC address: 00:21:5d:c8:0e:f2

Therefore, the MAC address of the infected Windows client is 00:21:5d:c8:0e:f2 (network-forensics/pcap-investigation-1/task 2.png)

Task 3
To determine the hostname of the infected system, the PCAP file was analyzed using Wireshark.

Since the infected host had already been identified as 10.1.21.58, a display filter was applied to isolate NetBIOS Name Service (NBNS) traffic associated with this IP address:

nbns && ip.addr == 10.1.21.58

NBNS traffic is commonly used by Windows systems to register and resolve hostnames within a local network.

The filtered results showed several NBNS Registration packets originating from the infected host. In the packet details under NetBIOS Name Service, the registered name was observed as:

DESKTOP-ES9F3ML

This value represents the hostname of the infected Windows client.

Answer: DESKTOP-ES9F3ML (network-forensics/pcap-investigation-1/task 3.png)

Task 4
Focused on Kerberos authentication traffic to extract user credentials, applying the display filter:

kerberos.CNameString

Examined the Kerberos packets and located the Client Name field, which revealed the username:

answer: gwyatt (network-forensics/pcap-investigation-1/task 4.png)

Task 5
Focused on SAMR (Security Account Manager Remote) and SMB2 traffic, which contains user account information.

Applied the display filter:

samr

or specifically looked at QueryUserInfo response packets.
In the packet details pane, expanded the fields under:

samr.samr_UserInfo2:full_name

Located the Full Name field, which revealed:

Gabriel Wyatt

answer: Gabriel Wyatt (network-forensics/pcap-investigation-1/task 5.png)

Task 6
I applied the following display filter to show only HTTP requests and TLS Client Hello packets containing SNI:

(http.request or tls.handshake.extensions_server_name) and !(ssdp)

After applying the filter, the packet list showed only traffic where hostnames are visible. I selected each TLS Client Hello and expanded the Server Name Indication (SNI) field to extract the domain names. One example visible in my capture is:
answer:
communicationfirewall-security[.]cc
holiday-forever[.]cc 
(network-forensics/pcap-investigation-1/task 6.png)

