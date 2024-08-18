Google Cybersecurity Certificate
---
---
## Network Traffic Analysis
> In this scenario, you are a cybersecurity analyst at a company that provides IT services. Several clients reported that they could not access a specific website and encountered the "destination port unreachable" error. Your task is to analyze the network issue and identify the affected protocol. To begin, you attempt to access the website and experience the same error. To investigate further, you use the network analyzer tool tcpdump while attempting to reload the webpage. The browser sends a DNS query using the UDP protocol to obtain the IP address of the domain. This IP address is then used in an HTTPS request to the web server. However, the network analyzer reveals that when UDP packets are sent to the DNS server, ICMP packets return an error message: "udp port 53 unreachable." Your analysis will focus on understanding the impact of these DNS and ICMP interactions on the network's security and identifying the underlying issue.
>
> Below is the tcpdump log <br/>
> ![tcpdump log](../images/networktrafficanalyzer.png)

### Overview
The UDP protocol reveals that the DNS server for the website whose domain name is yummyrecipeforme.com is facing an error.

This is based on the results of the network analysis, which show that the ICMP echo reply returned the error message: “udp port 53 unreachable”.

Port 53 of UDP is used for receiving DNS requests and returning an IP address of the corresponding website.

This may indicate a problem with the DNS server that is hosting the website. It is possibly being flooded with a large number of requests, a type of Denial of Service (DoS) attack.

### Investigation and Analysis
At about 1:20 PM, many customers of our client company, Yummy Recipes For Me reported that they were unable to access the website.

Explain the actions taken by the IT department to investigate the incident: The IT department tried to access the website address and used the network analyzer tool to inspect the issue.

After sending UDP packets to the DNS server, ICMP packets are received indicating that port 53 was unreachable. Port 53 of it is already shut down and not receiving any requests. The issue is further evident by the plus sign flag following the query number 35084. There is also a flag associated with the DNS request for an A record, which maps a domain name to an IP address.  Our next steps include inspecting the server itself to find the source of flooding requests, troubleshooting and recovering the normal operation of the server. The DNS server can be down or traffic to port 53 is blocked by the firewall. The DNS server might be down due to a successful Denial of Service or a misconfiguration.

The IT department believes that a group of threats is trying to degrade the performance of the websites hosted by the DNS service provider and damage that DNS service provider. The threat actors are likely to be a competitor of that company.

## Network Attacks Analysis
> In this scenario, you are a security analyst for a travel agency that relies on its website to promote sales and vacation packages. One afternoon, your monitoring system alerts you to an issue with the web server. When you try to access the website, you encounter a connection timeout error. Using a packet sniffer, you discover that the web server is receiving an overwhelming number of TCP SYN requests from an unfamiliar IP address, indicating a potential SYN flood attack. The server is struggling to handle the excessive traffic, leading to service disruption. You temporarily take the server offline to allow it to recover and configure the firewall to block the suspicious IP address. However, you realize that this is a temporary solution, as the attacker could use IP spoofing to bypass the block. You must quickly inform your manager about the attack, discuss further actions to mitigate the threat, and prepare to explain the type of attack and its impact on the web server and company employees.
> 
> **Table: Wireshark TCP HTTPS logs** '
> ![networkattacks_1](../images/networkattacks_1.png)
> ![networkattacks_2](../images/networkattacks_2.png)
> ![networkattacks_3](../images/networkattacks_3.png)
> ![networkattacks_4](../images/networkattacks_4.png)

### Overview
One potential explanation for the website's connection timeout error message is: the web server is probably down and unable to respond to the coming requests, making them wait for a long time.

The logs show that: a suspicious device with an external IP address 203.0.113.0 kept sending synchronize request to the web server making the server resource unavailable and causing traffic problem for other requests. The issue happened in the transport layer of TCP/IP model where the TCP is responsible for transmitting data packets.

This event could be a successful Smurf attack causing DoS as it is from a single device.

### Explanation and Possible Measurement
When website visitors try to establish a connection with the web server, a three-way handshake occurs using the TCP protocol.
1.	The visitors’ devices send SYN packets to the web server asking for connection.

2.	The [SYN, ACK] packet is sent as a response of the server accepting the visitor’s request and reserve resources for the final step of the handshake.

3.	The [ACK] packet sent from the visitor’s machine acknowledging the permission to connect.

The first SYN request of the malicious actor was already accepted but that device kept sending a large number of SYN packets aiming to flood the server and slow the traffic. The server’s resources became unavailable, overwhelmed and finally unable to respond to the requests.

Explain what the logs indicate and how that affects the server:
The logs indicate that the server struggled to respond to the legitimate requests from the employees. The [RST, ACK] packets were sent to the requesting visitor, and they received a timeout error message as the [SYN, ACK] requests were not received by the server. The server finally stopped responding to any request (from log item number 125)

This is probably because the malicious actor spoofed an authorized IP address to conduct a DoS attack targeting the web server.

The next step is to investigate the IP address that flooded the server and send a report to the authorities.

One possible measure is to configure the server’s firewall to block any suspicious IP address automatically. For instance, familiar IP address but suddenly from a strange geographical location. Installing a new generation firewall (NGFW) can monitor any unusual traffic on the network. NGFW includes features that detect network anomalies to ensure that oversized broadcasts are detected before they have chance to bring down the network.
