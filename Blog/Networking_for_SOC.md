# Networking for Security Analysts


## Understanding the Basics of Networking

For experienced Security Operations Center (SOC) analysts, a robust comprehension of networking fundamentals is paramount. Our daily tasks of detecting, investigating, and preventing security incidents are deeply intertwined with the intricacies of how network communications function. This chapter lays the groundwork by exploring these essential concepts, starting with the Open Systems Interconnection (OSI) model and progressing through packet encapsulation and transfer mechanisms. This foundational knowledge will be critical as we delve into specific security systems in subsequent chapters.

### The Open Systems Interconnection (OSI) Model

The OSI model is a conceptual framework that standardizes the functions of a telecommunication or computing system in terms of the communication functions of its components.[^1] It provides a layered approach, dividing the complex process of network communication into seven distinct layers, each with specific responsibilities. Understanding these layers is crucial for deciphering network traffic, identifying anomalies, and comprehending the operational context of various security technologies.

![alt text](images/1.png)
OSI Layer Model Graphical representation [^1]

The seven layers of the OSI model, from the lowest to the highest, are:

1.  **Physical Layer:** This is the foundational layer, defining the electrical and physical specifications for data connections. It deals with the transmission and reception of unstructured raw data as a stream of bits over a physical medium. This layer specifies characteristics such as voltage levels, cable specifications (e.g., copper, fiber optic), connector types, and wireless frequencies. It is concerned with the physical signaling and the media through which data is transmitted. Low-level networking equipment operates at this layer, and it is not concerned with protocols or higher-layer items.

2.  **Data Link Layer:** This layer is responsible for the reliable transfer of data frames between two directly connected nodes across the physical layer. It handles physical addressing (MAC addresses), framing, error detection, and sometimes flow control. The data link layer is often divided into two sub-layers: the Media Access Control (MAC) layer, which controls how devices on the same network segment share the medium, and the Logical Link Control (LLC) layer, which provides an interface to the network layer. Technologies such as Ethernet, Wi-Fi (802.11), and protocols like the Address Resolution Protocol (ARP) operate at this layer. The Data Link Layer packages data into frames for transmission over the physical medium.

3.  **Network Layer:** The primary function of the network layer is to handle packet routing via logical addressing (IP addresses) and switching functions, enabling data to travel across multiple networks. This layer is responsible for path determination, logical addressing (assigning IP addresses), and packet forwarding. If a message is too large, the network layer may handle fragmentation (splitting packets) at the source and reassembly at the destination. Key protocols at this layer include the Internet Protocol (IP), Internet Control Message Protocol (ICMP), and Internet Group Management Protocol (IGMP). Both IPv4 (32-bit) and IPv6 (128-bit) addresses are defined at this layer.

4.  **Transport Layer:** The transport layer provides end-to-end connections between hosts and ensures the reliable and ordered delivery of data. It handles segmentation and reassembly of data, as well as error control and flow control to ensure data integrity. The transport layer provides quality of service (QoS) functions. The two main protocols at this layer are the Transmission Control Protocol (TCP), which is connection-oriented and provides reliable, ordered delivery, and the User Datagram Protocol (UDP), which is connectionless and provides faster but less reliable delivery. Protocols like Secure Sockets Layer (SSL) and Transport Layer Security (TLS) can also operate at this layer to provide encryption. The Transport Layer breaks down data into segments (for TCP) or datagrams (for UDP) for network transmission.

5.  **Session Layer:** This layer is responsible for establishing, managing, and terminating connections (sessions) between applications on different hosts. It handles authentication and authorization functions and ensures data is delivered as intended. The session layer is often explicitly implemented in applications using remote procedure calls.

6.  **Presentation Layer:** The presentation layer is concerned with data representation and encryption. It ensures that information sent by the application layer of one system is readable by the application layer of another system. This layer handles tasks such as data format conversion, character encoding, and data compression. It also deals with encryption and decryption of data for security purposes.

7.  **Application Layer:** This is the layer closest to the end user, providing communication functions directly to software applications. It interacts directly with applications and provides protocols for end-user services. Examples of protocols at this layer include the Domain Name System (DNS), Hypertext Transfer Protocol (HTTP), File Transfer Protocol (FTP), Simple Mail Transfer Protocol (SMTP), and Secure Shell (SSH). This layer defines how applications interact with the network.

### The TCP/IP Model

While the OSI model is a conceptual reference, the TCP/IP model is the protocol suite that underpins the internet and most modern networks. It is older than the OSI model and was designed to solve specific networking problems. The TCP/IP model has four layers, which can be mapped to the OSI model:

![alt text](images/2.jpg)
TCP/IP Model Layers Graphical representation [^1]

1.  **Application Layer:** This layer combines the functionalities of the OSI model's Application, Presentation, and Session layers. It provides protocols for application-level communication, such as HTTP, FTP, SMTP, DNS, and SSH.

2.  **Transport Layer:** This layer corresponds directly to the OSI model's Transport Layer. It provides end-to-end communication services, including TCP for reliable, connection-oriented communication and UDP for faster, connectionless communication.

3.  **Internet Layer:** This layer maps to the OSI model's Network Layer. Its primary responsibility is routing packets across networks. The main protocol at this layer is IP, along with supporting protocols like ARP, ICMP, and IGMP.

4.  **Network Access Layer (or Link Layer):** This layer combines the functionalities of the OSI model's Data Link and Physical layers. It handles the physical transmission of data and the interaction with the network hardware, encompassing technologies like Ethernet, Wi-Fi, and others responsible for placing and receiving TCP/IP packets on the network medium.

Understanding both models is beneficial. The OSI model provides a comprehensive theoretical framework, while the TCP/IP model reflects the practical implementation of internet protocols.

### Packet Encapsulation and Transfer

Network communication involves data originating from an application on one host being prepared for transmission across a network to an application on another host. This process involves encapsulation at the sending end and de-encapsulation at the receiving end. Data is broken down and encapsulated into packets, which are the fundamental units of data transfer across networks. Each packet contains several key components:

![alt text](images/3.jpg)
Data being encapsulated in a frame and transmitted over a network before being de-encapsulated at the receiving end [^1]

*   **Data:** This is the actual payload or the information being transmitted, originating from the application layer, which could be part of an email, a segment of a webpage, or any other form of digital content. This data is usually broken down in multiple packets for transfers and encapsulated with additional information to enable the actual data transfer. 
*   **Transport Layer Header:** This header, added by the Transport Layer, includes information such as source and destination port numbers, sequence numbers (for TCP), and checksums. It's responsible for end-to-end communication, ensuring data integrity and proper sequencing.
*   **Network Layer Header (IP Header):** Added by the Network Layer, this header contains the source and destination IP addresses, routing information, and the Time-to-Live (TTL) value. It's crucial for routing packets across different networks.
*   **Data Link Layer Header:** This header, added by the Data Link Layer, includes the source and destination MAC addresses, which are used for communication within the same local network segment.
*   **Data Link Layer Trailer:** Often included at the end of the frame, this trailer contains error detection information, such as the Frame Check Sequence (FCS), to ensure data integrity during transmission.

**Encapsulation, Transfer, and De-encapsulation:**

The process begins with **encapsulation**, where the application data is passed down through the layers of the TCP/IP model on the sending host. Each layer adds its respective header (and sometimes a trailer) to the data, forming a Protocol Data Unit (PDU). Once encapsulated into a frame at the Network Access Layer, the data is ready for **transfer** across the network. Switches and routers, operating at Layers 2 and 3 respectively, use MAC and IP addresses to forward frames and route packets. As the frame or packet traverses the network, intermediate devices examine the headers to make forwarding decisions. Upon reaching the receiving host, the process is reversed through **de-encapsulation**. Each layer removes its corresponding header (and trailer), passing the PDU up to the next higher layer until the application data is delivered to the receiving application.

### Logical and Physical Addressing

Understanding the difference between logical (IP) and physical (MAC) addresses is fundamental to comprehending network communication.

*   **MAC Addresses (Media Access Control):** These are physical addresses assigned to the network interface card (NIC) of a device. They are typically 48-bit hexadecimal addresses and are used for communication within the same local network segment (Layer 2). MAC addresses are unique within a local network and are used by switches to forward frames based on the destination MAC address.

*   **IP Addresses (Internet Protocol):** These are logical addresses assigned to devices on a network (Layer 3). IP addresses are hierarchical and are used for routing packets across different networks. Both IPv4 and IPv6 are used, with IPv4 being a 32-bit address and IPv6 being a 128-bit address. Routers use destination IP addresses to determine the path a packet should take to reach its destination network.

The ARP protocol plays a crucial role in resolving IP addresses to MAC addresses within a local network. When a host needs to send a packet to an IP address on the same local network, it uses ARP to determine the corresponding MAC address.

### Significance for SOC Analysts

A solid grasp of these networking fundamentals is essential for SOC analysts in numerous ways:

*   **Traffic Analysis:** Understanding packet headers and the functions of each layer allows analysts to interpret network traffic captures (e.g., PCAPs) effectively. This is crucial for identifying malicious activity, such as unusual protocols, suspicious ports, or anomalous traffic patterns.
*   **Security Device Operation:** Security devices like firewalls, Intrusion Detection/Prevention Systems (IDS/IPS), and Web Application Firewalls (WAFs) operate at different layers of the OSI model. Knowing the layer at which a device operates helps analysts understand its capabilities and limitations. For example, a traditional firewall primarily operates at Layers 3 and 4, while a WAF operates at Layer 7.
*   **Incident Investigation:** When investigating security incidents, understanding how network communication flows can help analysts trace the origin and destination of attacks, identify affected systems, and understand the scope of a breach. For instance, analyzing network logs and traffic can reveal lateral movement within a network.
*   **Rule and Policy Creation:** For managing security devices, analysts need to define rules and policies based on network protocols, IP addresses, ports, and application-layer information. A strong understanding of networking is essential for creating effective and accurate security rules.
*   **Threat Detection:** Recognizing normal network behavior and identifying deviations that might indicate malicious activity relies on understanding how legitimate network communication works. For example, understanding DNS queries and responses is crucial for detecting DNS exfiltration attempts.

### Understanding Traditional Firewall Systems

For many years, the firewall has served as the primary barrier between protected internal networks and potentially hostile external networks, playing a critical role in controlling the flow of network traffic and preventing unauthorized access.

**Historical Context and Core Functionality**

Traditional firewalls emerged in the late 1980s as a response to the growing need to protect internal resources from external threats. The earliest forms were packet filtering systems that operated by examining the headers of network packets. These firewalls made decisions about whether to permit or deny traffic based on a predefined set of rules that typically considered the source and destination IP addresses, port numbers, and network protocols (like TCP, UDP, ICMP). If a packet's header information matched a rule, the associated action (allow or block) was taken.[^2]

The fundamental objective of a traditional network firewall is to establish a secure boundary by separating a secured internal zone from a less secure external zone, meticulously controlling communications between the two. Without a firewall, any device with a public IP address would be directly accessible from the internet, significantly increasing the risk of attacks. Firewall policies define the specific types of traffic that are permitted to enter or leave the network, and any traffic that does not conform to these policies is blocked. This helps to prevent unauthorized users and malicious activities originating from less trusted zones.

![alt text](images/4.png)
Traditional firewall as a boundary between untrusted (external network) and trusted (internal network) zones[^2]

Traditional firewalls function as a checkpoint, inspecting and regulating network traffic based on the security rules configured by network administrators. Every piece of data traversing the internet or a network is encapsulated within network packets, as we discussed previously. Traditional firewalls analyze the header information of these packets against their configured rules to determine their legitimacy. Traffic that does not meet the specified criteria is blocked or dropped, ensuring that only safe and legitimate traffic is allowed to pass.

| Source Address | Source Port | Destination Address | Destination Port | Action | Explanation |
|----------------|-------------|---------------------|------------------|--------|-------------|
|   192.168.1.2 |           80 |         10.10.10.20 |               22 |  Allow | This rule allows traffic. Specifically, it permits connections originating from the IP address 192.168.1.2 on port 80 (typically HTTP) to the IP address 10.10.10.20 on port 22 (SSH). This suggests an internal machine (192.168.1.2) is allowed to connect to an external SSH server.
| 10.10.0.0 / 24 | Any | 192.168.0.0/24 | 443 | Deny | This rule denies traffic. It blocks any traffic coming from the IP range 10.10.0.0/24 to the IP range 192.168.0.0/24 on port 443 (typically HTTPS). This is a more restrictive rule, preventing a whole network from accessing a specific internal network's HTTPS services. The Any in the source port means any source port is blocked.
| Any | Any | Any | Any | Deny | This is a default deny rule. It's a crucial security best practice. It states that any traffic not explicitly allowed by a previous rule is blocked. This prevents unexpected or not configured traffic from passing through the firewall.

Table 1: Example traditional firewall rules for traffic management [^3]

**Stateless Packet Inspection**

One of the earlier methods employed by traditional firewalls for traffic filtering is **stateless packet inspection**. In this mode of operation, the firewall examines each network packet in isolation, without considering its relationship to any other packets or the overall state of a network connection. The decision to allow or block a packet is based solely on the information contained within that individual packet's header, such as the source and destination IP addresses and port numbers.[^4]

**Stateful Packet Inspection**

To address the limitations of stateless inspection, more advanced traditional firewalls implemented **stateful packet inspection**. A stateful firewall monitors the state of active network connections, keeping track of information such as the source and destination IP addresses and ports, the sequence numbers (for TCP), and the overall connection status. This information is maintained in a **state table**.[^4]

For instance, if an internal user initiates an HTTP request to an external web server (typically using TCP port 80 or 443), the stateful firewall will allow the initial outgoing packets based on the outbound rules. It will then create a state table entry for this connection, noting the source and destination IP addresses and ports, and the TCP sequence numbers. When the web server responds to this request, the firewall will examine the incoming packets and check if they match an existing entry in the state table. If a match is found (indicating that these incoming packets are part of the previously established connection initiated by the internal user), the packets are allowed through. However, unsolicited incoming packets that do not correspond to any active session in the state table are typically blocked.

Stateful inspection offers several advantages over stateless inspection:

*   **Enhanced Security:** By tracking the state of connections, stateful firewalls can better identify and block various types of attacks, such as connection hijacking and certain denial-of-service (DoS) attacks, that rely on manipulating the state of TCP sessions.
*   **More Granular Control:** Stateful firewalls can enforce more precise rules based on the connection state, allowing, for example, incoming traffic only in response to a previously initiated outgoing request.
*   **Simplified Rule Sets:** In some cases, stateful inspection can simplify firewall rule configuration. For instance, instead of needing explicit rules to allow all possible response packets for every type of outbound connection, a stateful firewall can automatically permit return traffic for established sessions.

Traditional packet filtering firewalls primarily operate at the **network layer (Layer 3)** of the OSI model. At this layer, they examine the headers of network packets, focusing on source and destination IP addresses. If the Firewall performs stateful packet inspection, then it operates at the **transport layer (Layer 4)** by tracking the state of active connections and ensuring that incoming traffic is part of an established session.

## Proxies and how they are different to firewalls: 
I often see a lot of confusion between the traditional role of the Firewall and the Proxy or Proxy Firewall. In this section I will briefly discuss their role in modern security architectures in a layered approach to provide comprehensive security.

### Understanding Proxy Server Functioning and it's part in the OSI Layer**

A proxy server functions as an intermediary, acting as a gateway between a user's device and the public internet. When a user requests a resource on the internet, the request is first directed to the proxy server. The proxy server then forwards this request to the destination server on behalf of the user and, upon receiving the response, relays the data back to the user's device. This indirect communication provides several key functionalities. Primarily, a proxy server can mask the user's actual IP address by using its own anonymous IP address when communicating with external servers, thereby enhancing user anonymity. Additionally, proxy servers often implement caching mechanisms, storing frequently accessed web content to reduce server load and improve latency for subsequent requests for the same resources. Some organizations also utilize proxy servers to enforce web access control policies, blocking or allowing traffic to specific URLs or content categories.

Crucially, a proxy server operates at the **application layer (Layer 7)** of the OSI model. At this layer, the proxy server has visibility into the application data being exchanged, such as HTTP/HTTPS requests and responses. This allows for deep packet inspection (DPI) at the application level, enabling the proxy to identify and potentially block malicious content or unauthorized application-specific activities. Due to their operation at the application layer, proxy firewalls are sometimes referred to as application firewalls or application-level gateways.[^5]

**Key Functional Differences Summarized**

| Feature             | Proxy Server                                                              | Traditional Firewall                                                        |
| ------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Primary Function    | Intermediary for user requests, anonymity, web access control, caching.   | Prevents unauthorized network access, filters traffic based on rules.       |
| OSI Layer           | Application Layer (Layer 7)                                               | Network Layer (Layer 3) and Transport Layer (Layer 4)                       |
| Traffic Handling    | Accepts and forwards application-level connection requests.               | Inspects and either allows or blocks network packets.                       |
| Visibility          | Deep visibility into application data (Layer 7).                          | Primarily focuses on network and transport layer headers (traditional)      |
| Direct Connection   | Prevents direct connections between internal users and external servers.  | Sits between networks, controlling the flow of traffic across boundaries.   |
| Anonymity           | Can hide the user's IP address.                                           | Typically does not provide anonymity on their own.                          |
| Caching             | Often provides caching of web content.                                    | Generally does not provide caching (proxy firewalls being an exception).    |
| Threat Prevention   | Focuses on application-level threats and controlling web access.          | provide limited threat prevention, can implement Allow or Blocklist based on traffic |

In essence, while both proxy servers and firewalls contribute to network security, they operate at different layers of the OSI model and fulfill distinct roles. Proxy servers act as forward or reverse proxies at the application level, mediating and inspecting specific application traffic, whereas firewalls, especially traditional ones, operate at lower network layers to control broader network traffic based on defined rules.

## Web application firewalls (WAFs)

## Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS)

Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS) are crucial network security technologies designed to identify and potentially respond to malicious activities targeting a network or its systems. While both aim to enhance security by monitoring network traffic, they differ significantly in their response to detected threats.

An **IDS** is primarily a monitoring system that analyzes network traffic for suspicious patterns or anomalies that may indicate an ongoing attack or a security policy violation.. Critically, an IDS operates out of the main traffic flow and does not actively interfere with data transmission. It acts as a passive observer, analyzing a mirrored copy of network traffic to preserve network performance.[^6]

In contrast, an **IPS** takes a more active role in network security. Similar to an IDS, an IPS analyzes network traffic for malicious activities and vulnerabilities. However, when a threat is identified, an IPS can take automated actions to prevent the threat from compromising the network. These preventative measures can include blocking the malicious traffic, dropping harmful data packets, blocking the source IP address, resetting the connection, or triggering other security mechanisms. An IPS is typically deployed in the direct path of network traffic (inline), allowing it to scrutinize and act on threats in real-time.[^7]

![alt text](images/5.webp)
Difference between IDS and IPS deployed within a network [^7]

| 	                                | IPS Deployment                                        |  	IDS Deployment              |
|-----------------------------------|-------------------------------------------------------|-------------------------------|              
|Placement in Network Infrastructure| Part of the direct line of communication (inline)     |Outside direct line of communication (out-of-band)|
|System type                        | Active (monitor & automatically defend) and/or passive|Passive (monitor & notify)     |
|Detection Mechanisms 	            |1. Statistical anomaly-based detection<br>2. Signature detection:<br>- Exploit-facing signatures<br>- Vulnerability-facing signatures|1. Signature detection:<br>- Exploit-facing signatures

Table 3 - Difference between IDS and IPS systems [^6]

### Differences Between IDS/IPS and Firewalls

While firewalls, IDS, and IPS are all essential components of a comprehensive network security framework, they serve distinct purposes and operate differently.

In contrast to the Firewall that acts as a barrier or filter that controls network traffic flow, IDS and IPS are specifically designed to detect and respond to malicious activities that may bypass firewall controls or originate from within the network. While a firewall blocks traffic based on rules applied to addresses and ports, an IDS/IPS examines the actual content and behavior of network traffic to identify potential threats.[^7]

The placement of these security solutions within the network architecture also differs. A firewall is typically located at the network perimeter as the initial line of defense. An IPS is often positioned behind the firewall to examine traffic that has already passed the initial filtering stage, allowing it to analyze and act on potentially malicious data before it reaches internal resources. An IDS, being a passive monitoring system, is often placed out-of-band, mirroring network traffic for analysis without being directly in the communication path.[^7]

### Purpose of Intrusion Detection Systems

The fundamental purpose of an IDS is to identify potential security breaches, malicious activities, or policy violations occurring within a network or on its systems. By monitoring network traffic and system activities, an IDS aims to detect suspicious patterns, known attack signatures, and deviations from normal behavior.

Specifically, the purposes of an IDS include :

*   **Detecting Exploits:** Identifying attempts to leverage vulnerabilities in applications or operating systems.
*   **Identifying Attack Patterns:** Recognizing known sequences of actions associated with malicious activities, such as network scans or denial-of-service (DoS) attacks.
*   **Monitoring for Anomalous Behavior:** Detecting unusual network traffic patterns or communication that deviate from established baselines, which could indicate the presence of malware or unauthorized activity.
*   **Providing Alerts and Logs:** Generating notifications and maintaining records of detected suspicious events, enabling security analysts to investigate and respond to potential incidents.
*   **Identifying Configuration Issues:** Detecting potential bugs or problems with device configurations that could create security weaknesses.

### Understanding IDS/IPS Types and Their Differences in the Detection Stack

**1. Network-based Intrusion Detection/Prevention System (NIDS/NIPS)**

A Network-based IDS (NIDS) operates by monitoring network traffic across an entire protected network or specific network segments. Typically deployed at strategic points within the network infrastructure, such as the ingress and egress points of internet traffic or at key switching locations, NIDS examines the flow of data to and from devices. By analyzing packet contents and metadata, NIDS aims to identify suspicious patterns and sequences that match known attack signatures or deviate from established baselines of normal network behavior.

**2. Host-based Intrusion Detection/Prevention System (HIDS/HIPS)**

A Host-based IDS (HIDS) is deployed on a specific endpoint or host within the network, such as a server, workstation, or other critical asset. Unlike NIDS, which monitors network traffic, HIDS focuses on the activities occurring on the individual host. This includes monitoring system logs, file system integrity, process activity, and network traffic originating from or destined for the specific host. When suspicious activity is detected, such as unauthorized modifications to critical system files or unusual process execution, the HIDS generates alerts for local administrators or a central security management system. Similar to NIDS, a traditional HIDS is primarily a monitoring and alerting tool and does not typically block malicious actions.

**3. Protocol-based Intrusion Detection System (PIDS)**

A Protocol-based IDS (PIDS) is typically positioned in front of a server, such as a web server, and focuses on monitoring and analyzing the communication protocols being used between a user or device and the server. The PIDS examines the behavior and state of the specific protocol to identify any deviations from expected or acceptable patterns. For instance, a PIDS monitoring HTTP traffic might look for malformed requests, attempts to exploit known web server vulnerabilities, or violations of HTTP protocol standards.

By focusing on the specifics of a particular protocol, PIDS can provide more granular and context-aware detection of attacks targeting services utilizing that protocol. They are often tailored to monitor protocols like HTTP, SMTP, FTP, or DNS and can be effective in identifying application-layer attacks that may not be easily detected by more general network-based or host-based systems.

**4. Application Protocol-based Intrusion Detection System (APIDS)**

An Application Protocol-based IDS (APIDS) takes an even deeper dive into the application layer by focusing on the specific protocols used by particular applications. Typically residing within the server-side of an application, an APIDS monitors and interprets the communication within application-specific protocols. A common example is an APIDS that monitors the SQL protocol used for communication between a web server and a database. By understanding the expected syntax, commands, and data flow within the application protocol, the APIDS can detect anomalous or malicious activity, such as SQL injection attempts or unauthorized data access. This type of IDS has very similar functionality to the already discussed Web Application Firewall (WAF)

**5. Hybrid Intrusion Detection/Prevention System**

A Hybrid Intrusion Detection System combines two or more of the aforementioned IDS approaches to provide a more comprehensive and integrated view of security threats. By integrating information from network-based sensors, host-based agents, and potentially protocol-specific monitors, a hybrid IDS can correlate events and gain a broader understanding of suspicious activities across different layers of the IT infrastructure. For example, a hybrid system might correlate a network-based alert indicating suspicious traffic with host-based logs showing anomalous process activity on a specific endpoint, providing stronger evidence of a potential compromise. An example of a hybrid approach is the integration of network traffic analysis with endpoint detection and response (EDR) data for a more holistic view of potential threats.

### Different Types of Detections

IDS and IPS solutions employ various methods to detect malicious activities, with the most common being signature-based detection and anomaly-based detection. IPS may also utilize policy-based detection.

*   **Signature-Based Detection:** This method relies on a database of pre-defined signatures or patterns of known attacks and vulnerabilities (rules). The IDS/IPS analyzes network traffic and compares it against these signatures. If a match is found, an alert is triggered (IDS) or the traffic is blocked (IPS). Signature-based detection is effective at identifying known threats with high accuracy. However, it is limited in its ability to detect new or zero-day attacks for which signatures do not yet exist. Examples of signature-based rules often look for specific network traffic patterns associated with known malware or exploit attempts. Many traditional IDS/IPS systems primarily rely on signature-based detection.

*   **Anomaly-Based Detection:** This method establishes a baseline of normal network behavior by analyzing various network metrics and traffic characteristics over time using techniques like machine learning. Once a baseline is established, the IDS/IPS continuously monitors network traffic and identifies any deviations or anomalies from this normal behavior. Detected anomalies are flagged as potentially malicious. Anomaly-based detection can be effective at detecting new and unknown threats, as it doesn't rely on pre-existing signatures. However, it is more prone to generating false positives, as legitimate but unusual network activity can be misidentified as malicious. Modern IDS/IPS and especially Network Detection and Response (NDR) solutions increasingly incorporate anomaly-based detection capabilities.

*   **Policy-Based Detection:** This detection method, primarily used by IPS, relies on predefined security policies configured by administrators. The IPS monitors network traffic to ensure it complies with these established policies. Any traffic that violates a defined policy triggers a preventative action. Policies can be based on various criteria, such as acceptable application usage, allowed communication protocols, or restrictions on specific types of network behavior.

### Example of an IDS Signature (Rule)

Creating high-quality and specific detection rules is crucial for effective threat detection while minimizing noise. Many open-source and commercial IDS/IPS systems utilize a rule-based language (e.g., Snort, Suricata).

Let's consider a simplified example of a signature-based IDS rule ([from the SNORT IDS/IPS community ruleset](https://www.snort.org/downloads/#rule-downloads)) designed to detect attempts to exploit a specific vulnerability in a web server:

```
alert tcp $EXTERNAL_NET any -> $HOME_NET $HTTP_PORTS 
(msg:"SERVER-OTHER Apache Log4j logging remote code execution attempt"; 
flow:to_server,established;
http_uri; 
content:"${jndi:",fast_pattern,nocase;
metadata:policy balanced-ips drop,policy connectivity-ips drop,policy max-detect-ips drop,policy security-ips drop,ruleset community;
service:http;
reference:cve,2021-44228; reference:cve,2021-44832; reference:cve,2021-45046; reference:cve,2021-45105; 
classtype:attempted-user; sid:58722; rev:5;)
```

**Breakdown of the Rule:**
This Snort rule is designed to detect attempts to exploit the Log4j vulnerability (CVE-2021-44228 and related CVEs). Let's break down the rule:

`alert tcp $EXTERNAL_NET any -> $HOME_NET $HTTP_PORTS`: This line specifies the type of alert (TCP), the source network ($EXTERNAL_NET, representing any external network), any port on the source, the destination network ($HOME_NET, representing your internal network), and the destination ports ($HTTP_PORTS, representing common HTTP ports like 80 and 443). This means the rule triggers on TCP traffic coming from outside your network to HTTP ports within your network.

`msg:"SERVER-OTHER Apache Log4j logging remote code execution attempt"`";: This is the message that will be logged if the rule triggers. It indicates a potential Log4j exploit attempt targeting an Apache server.

`flow:to_server,established`: This specifies that the rule only applies to traffic flowing towards the server and that a connection has already been established.

`http_uri`: This ensures that the rule only applies to HTTP requests, checking the URI part of the HTTP request.

`content:"${jndi:",fast_pattern,nocase`: This is the core of the detection. It looks for the string "${jndi:" within the HTTP request URI. This string is a common indicator of Log4j exploitation attempts using JNDI lookup. fast_pattern optimizes the search, and nocase makes the search case-insensitive.

`metadata`: This section adds metadata to the rule. The policy directives indicate that if this rule triggers, the traffic should be dropped by various intrusion prevention systems (IPS). ruleset community indicates the rule comes from a community-contributed ruleset.

`service:http`: This further refines the rule to only apply to HTTP traffic.

`reference`:cve,2021-44228; reference:cve,2021-44832; reference:cve,2021-45046; reference:cve,2021-45105;: These lines provide references to the relevant Common Vulnerabilities and Exposures (CVEs) associated with the Log4j vulnerability.

`classtype:attempted-user`: This classifies the event as an attempted user-level attack.

`sid:58722; rev:5;`: sid is the signature ID, a unique identifier for this rule. rev is the revision number, indicating updates to the rule.

In short, this rule acts as a safeguard against Log4j exploits by monitoring incoming HTTP traffic and looking for the telltale `"${jndi:"` string in the URI. If found, it logs the event and, based on the defined policies, may drop the malicious connection.

### Challenges with IDS/IPS

Despite their importance, IDS/IPS systems face several challenges:[^8]

*   **Alert Overload and False Positives:** Traditional IDS/IPS, especially signature-based systems, can generate a high volume of alerts, many of which may be false positives (legitimate activity incorrectly identified as malicious). This can lead to alert fatigue among security analysts, making it difficult to identify genuine threats. Tuning rules to reduce false positives often requires significant expertise.
*   **Lack of Context:** IDS alerts often lack the necessary context to understand the full scope and severity of a potential attack. Correlating IDS alerts with events from other security solutions (e.g., EDR, firewalls) is crucial but can be time-consuming.
*   **Difficulty Detecting Zero-Day Attacks:** Signature-based IDS/IPS are ineffective against new, previously unknown (zero-day) attacks because there are no signatures available for them. Attackers often develop new exploits that evade existing signatures.
*   **Evasion Techniques:** Attackers employ various techniques to evade detection by IDS/IPS, such as fragmentation, flooding, obfuscation, and encryption. Encrypted traffic, in particular, poses a challenge for traditional IDS/IPS unless decryption is performed.
*   **Performance Impact (IPS):** Because IPS operates inline and actively analyzes traffic, it can potentially introduce latency and impact network performance, especially under heavy load. Careful planning and properly sized hardware are necessary to mitigate this impact.
*   **Complexity and Management Overhead:** Configuring and managing IDS/IPS rules and policies can be complex and require skilled analysts. Keeping signature databases and detection logic up-to-date is also an ongoing task.
*   **Limited Visibility:** Traditional IDS/IPS deployed at the network perimeter may have limited visibility into lateral movement of threats within the internal network after an initial compromise. They also often lack sufficient visibility into cloud environments, SaaS applications, and encrypted communications.
*   **Nonspecific / Too broad Rules:** Generic, broad rules can trigger on common benign activities, leading to noise, while highly specific rules might miss variations of attacks. Balancing specificity and coverage is a constant challenge in rule writing.

Due to these challenges and the evolving threat landscape, organizations are increasingly adopting more sophisticated solutions like Network Detection and Response (NDR) platforms, which often incorporate IDS/IPS functionalities along with advanced analytics, anomaly detection, and broader visibility across modern IT environments. NDR aims to provide more contextualized alerts, better detection of advanced and insider threats, and improved incident response capabilities. While IDS/IPS remain valuable components of a security strategy, understanding their limitations is crucial for building a robust and effective defense-in-depth approach.[^9]

## Network Detection and Response - NDR

## Next Generation Firewall - NGFW

# References

Aztech IT. (2023, December 14). *Next-generation firewall (NGFW) vs traditional firewall*. Aztechit.co.uk. [https://www.aztechit.co.uk/blog/next-generation-firewall-ngfw-vs-traditional-firewall](https://www.aztechit.co.uk/blog/next-generation-firewall-ngfw-vs-traditional-firewall)

Check Point Software Technologies. *Next-generation firewall vs. traditional firewall - Check Point Software*. Checkpoint.com. [https://www.checkpoint.com/cyber-hub/network-security/what-is-next-generation-firewall-ngfw/next-generation-firewall-vs-traditional-firewall/](https://www.checkpoint.com/cyber-hub/network-security/what-is-next-generation-firewall-ngfw/next-generation-firewall-vs-traditional-firewall/)

Cloudflare. *What is a WAF? | Web application firewall explained*. Cloudflare.com. [https://www.cloudflare.com/learning/ddos/glossary/web-application-firewall-waf/](https://www.cloudflare.com/learning/ddos/glossary/web-application-firewall-waf/)

Corelight. *NDR vs. IDS: Which is best for threat detection?* Corelight.com. [https://corelight.com/resources/glossary/ndr-vs-ids](https://corelight.com/resources/glossary/ndr-vs-ids)

Darktrace Threat Research Team, Nobregas, N., Foulger, E., & Trail, R. (2024). *Detecting & investigating lateral movement*. Darktrace.com. [https://darktrace.com/blog/a-security-analysts-view-detecting-and-investigating-lateral-movement-with-darktrace](https://darktrace.com/blog/a-security-analysts-view-detecting-and-investigating-lateral-movement-with-darktrace)

ExtraHop. (2019, February 7). *NDR vs. IPS for intrusion prevention, detection, and response*. Extrahop.com. [https://www.extrahop.com/blog/network-detection-response-vs-intrusion-prevention-systems](https://www.extrahop.com/blog/network-detection-response-vs-intrusion-prevention-systems)

ExtraHop. (2025, January 29). *Investigating a data leak with Reveal(x) | ExtraHop*. Extrahop.com. [https://www.extrahop.com/blog/investigating-fake-chrome-extension-postman-part-1](https://www.extrahop.com/blog/investigating-fake-chrome-extension-postman-part-1)

F5. *What is a web application firewall (WAF)?* F5.com. [https://www.f5.com/glossary/web-application-firewall-waf](https://www.f5.com/glossary/web-application-firewall-waf)

Fortinet. *WAF vs firewall: Web application and network firewalls*. Fortinet.com. [https://www.fortinet.com/resources/cyberglossary/waf-vs-firewall](https://www.fortinet.com/resources/cyberglossary/waf-vs-firewall)

Fortinet. *What is network detection and response (NDR)?* Fortinet.com. [https://www.fortinet.com/resources/cyberglossary/what-is-ndr](https://www.fortinet.com/resources/cyberglossary/what-is-ndr)

[^4] FS Community. *NGFW vs. traditional firewall: What’s the difference.* Community.fs.com. [https://community.fs.com/article/ngfw-vs-traditional-firewall-whats-the-difference.html](https://community.fs.com/article/ngfw-vs-traditional-firewall-whats-the-difference.html)

[^1] FS.com (FS), (2022, April 1). *TCP/IP vs. OSI: What’s the difference between them?* [https://www.fs.com/blog/tcpip-vs-osi-whats-the-difference-between-the-two-models-1446.html.](https://www.fs.com/blog/tcpip-vs-osi-whats-the-difference-between-the-two-models-1446.html.)

Naidu, K. (2000). *Firewall checklist* (SANS Institute). SANS Institute. [https://www.sans.org/media/score/checklists/FirewallChecklist.pdf](https://www.sans.org/media/score/checklists/FirewallChecklist.pdf)

[^7] Palo Alto Networks. *IPS. vs. IDS vs. Firewall: What are the differences?* Paloaltonetworks.com. [https://www.paloaltonetworks.com/cyberpedia/firewall-vs-ids-vs-ips](https://www.paloaltonetworks.com/cyberpedia/firewall-vs-ids-vs-ips)

[^3] Palo Alto Networks. *What are firewall rules? | Firewall rules explained*. Paloaltonetworks.com. [https://www.paloaltonetworks.com/cyberpedia/what-are-firewall-rules](https://www.paloaltonetworks.com/cyberpedia/what-are-firewall-rules)

[^5] Palo Alto Networks. *What is a Proxy Firewall? | Proxy firewall definition*. [https://www.paloaltonetworks.com/cyberpedia/what-is-a-proxy-firewall](https://www.paloaltonetworks.com/cyberpedia/what-is-a-proxy-firewall)

[^2] Palo Alto Networks. *What is a firewall? | Firewall definition*. Paloaltonetworks.com. [https://www.paloaltonetworks.com/cyberpedia/what-is-a-firewall](https://www.paloaltonetworks.com/cyberpedia/what-is-a-firewall)

[^6] Palo Alto Networks. *What is an intrusion detection system?* Paloaltonetworks.com. [https://www.paloaltonetworks.com/cyberpedia/what-is-an-intrusion-detection-system-ids](https://www.paloaltonetworks.com/cyberpedia/what-is-an-intrusion-detection-system-ids)

[^8] Stamus Networks. (2023, November 28). *What is the difference between IDS/IPS and NDR?* Stamus-networks.com. [https://www.stamus-networks.com/blog/what-is-the-difference-between-ids/ips-and-ndr](https://www.stamus-networks.com/blog/what-is-the-difference-between-ids/ips-and-ndr)

Trend Micro. *What is network detection and response (NDR)?* Trendmicro.com. [https://www.trendmicro.com/en_us/what-is/xdr/ndr.html](https://www.trendmicro.com/en_us/what-is/xdr/ndr.html)

[^9] Vectra AI. (2023). *Why security teams are replacing IDS with NDR*. Vectra AI. [https://cdn.prod.website-files.com/64e50cbe2b6f932c04238c14/6630b7eeb51ce629c0f4a006_Why-Security-Teams-are-Replacing-IDS-with-NDR.pdf](https://cdn.prod.website-files.com/64e50cbe2b6f932c04238c14/6630b7eeb51ce629c0f4a006_Why-Security-Teams-are-Replacing-IDS-with-NDR.pdf)

**Last Accessed:** 6th of April 2024.
