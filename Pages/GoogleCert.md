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
