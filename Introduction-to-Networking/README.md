# Introduction to Networking

## Networking Overview

A network enables two computers to communicate with each other. There is a wide array of topologies (mesh/tree/star), mediums (ethernet/fiber/coax/wireless), and protocols (TCP/UDP/IPX) that can be used to facilitate the network. It is important for security professionals to understand networking because when the network fails, the error may be silent, causing us to miss something.

Setting up a large, flat network is not extremely difficult, and it can be a reliable network, at least operationally. However, a flat network is like building a house on a land plot and considering it secure because it has a lock on the door. By creating lots of smaller networks and having them communicate, we can add defense layers. Pivoting around a network is not difficult, but doing it quickly and silently is tough and will slow attackers down.

![Story Time: Subnetting](assets/1-story-time-subnetting.png)

### Basic Information

Let us look at the following high-level diagram of how a Work From Home setup may work.

![High-Level Diagram](assets/2-HighLevel-Diagram.png)

The entire internet is based on many subdivided networks, as shown in the example and marked as "Home Network" and "Company Network." We can imagine networking as the delivery of mail or packages sent by one computer and received by another.

Suppose we want to visit a company's website from our "Home Network." In that case, we exchange data with the company's website located in their "Company Network." As with sending mail or packets, we know the address where the packets should go. The website address or Uniform Resource Locator (URL) which we enter into our browser is also known as Fully Qualified Domain Name (FQDN).

The difference between URLs and FQDNs is that:

- An FQDN (e.g., `www.hackthebox.eu`) only specifies the address of the "building."
- A URL (e.g., `https://www.hackthebox.eu/example?floor=2&office=dev&employee=17`) also specifies the "floor," "office," "mailbox," and the corresponding "employee" for whom the package is intended.

We will discuss the exact representations and definitions more clearly and precisely in other sections.

The fact is that we know the address, but not the exact geographical location of the address. In this situation, the post office can determine the exact location, which then forwards the packets to the desired location. Therefore, our post office forwards our packets to the main post office, representing our Internet Service Provider (ISP).

Our post office is our router, which we utilize to connect to the "Internet" in networking.

As soon as we send our packet through our post office (router), the packet is forwarded to the main post office (ISP). This main post office looks in the address register/phonebook (Domain Name Service) where this address is located and returns the corresponding geographical coordinates (IP address). Now that we know the address's exact location, our packet is sent directly there by a direct flight via our main post office.

After the web server has received our packet with the request for their website, the web server sends us back the packet with the data for the presentation of the website via the post office (router) of the "Company Network" to the specified return address (our IP address).


## Extra Points

In the high-level diagram, ideally, the company network would consist of five separate networks to enhance security and manageability. Here's a breakdown of the suggested network segmentation:

1. **DMZ (Demilitarized Zone)**
   - **Purpose**: The Web Server should be placed in a DMZ. The DMZ is a separate network that acts as a buffer between the public internet and the internal network. Since clients on the internet can initiate communications with the website, the web server is more exposed and likely to be compromised. By isolating it in a DMZ, administrators can implement additional security measures and networking protections to safeguard the rest of the network from potential threats.

2. **Workstations**
   - **Purpose**: Workstations should be on their own network. Ideally, each workstation should have a Host-Based Firewall rule to prevent it from communicating with other workstations. This setup reduces the risk of attacks like spoofing or man-in-the-middle attacks, which become more concerning if workstations share the same network with servers.

3. **Administration Network**
   - **Purpose**: The Switch and Router should be on an "Administration Network." This network is dedicated to administrative tasks and management of network devices. By separating administration traffic from other types of network traffic, you prevent workstations from snooping on or interfering with communication between these devices. For instance, I've often observed OSPF (Open Shortest Path First) advertisements during penetration tests. Without a trusted network for administrative tasks, anyone on the internal network could potentially send malicious advertisements and perform man-in-the-middle attacks.

4. **IP Phones**
   - **Purpose**: IP Phones should be on their own network. This segmentation is crucial for both security and performance reasons. By isolating IP phones, you prevent computers from eavesdropping on phone communications. Additionally, IP phones are sensitive to latency, so having a dedicated network allows administrators to prioritize voice traffic and minimize lag.

5. **Printers**
   - **Purpose**: Printers should be on their own network as well. While it may seem unusual, printers are notoriously difficult to secure. For example, Windows operating systems might perform NTLMv2 authentication during print jobs, which can lead to password theft. Furthermore, printers often handle sensitive information and can be a target for persistence attacks. Isolating them on a separate network helps mitigate these risks.

By following these practices, you can enhance network security, performance, and manageability, providing a more robust defense against potential threats and ensuring efficient operation of network resources.

- Internet VS Intranet VS Extranet
The internet is a public network accessible to anyone, while intranets are private networks accessible only to authorized users within an organization. Extranets are private networks that allow external parties to access certain parts of an organization's intranet.


## Network Types

Each network is structured differently and can be set up individually. For this reason, various types and topologies have been developed to categorize these networks. Understanding network types can be overwhelming due to the vast range of terminologies, some of which include geographical aspects. While some terms may not be frequently used in practice, they are still valuable to know, especially for specific scenarios or exams. This section will cover Common Terms and Book Terms. 

### Common Terminology

| Network Type                   | Definition                                          |
|-------------------------------|-----------------------------------------------------|
| **Wide Area Network (WAN)**   | The Internet; connects multiple LANs over large distances. |
| **Local Area Network (LAN)**  | Internal networks within a limited area, such as a home or office. |
| **Wireless Local Area Network (WLAN)** | Internal networks that are accessible over Wi-Fi. |
| **Virtual Private Network (VPN)** | Connects multiple network sites to create a single, secure LAN over the internet. |

### Book Terms

Book terms can be useful for understanding specific network concepts and are sometimes referenced in networking literature and exams. While they might not come up in everyday use, they can be critical in certain technical situations. For example, knowing these terms could be important if you encounter rare issues, such as an email server failing to deliver emails beyond a specific geographical distance.

**Note**: Don't feel pressured to memorize all book terms unless you're preparing for a networking certification or exam. Understanding the common terminology is usually sufficient for practical purposes.

## Network Types

Each network type is structured differently and serves distinct purposes. Understanding these types helps in managing and securing networks effectively. Below are the common and book terms related to different network types.

### WAN (Wide Area Network)

The WAN, commonly referred to as The Internet, is a network that covers a large geographic area, connecting multiple LANs (Local Area Networks). Here are key points about WANs:

- **WAN Address**: The address accessible over the Internet.
- **Internal WAN**: Large organizations or government agencies might have their own WAN, also known as an Intranet or Airgap Network.
- **Identification**: WANs are identified using WAN-specific routing protocols like BGP (Border Gateway Protocol) and IP addresses that fall outside the RFC 1918 range (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

### LAN / WLAN (Local Area Network / Wireless Local Area Network)

LANs and WLANs are networks that serve internal communications within a limited area, like a home or office:

- **LAN**: Uses IP addresses designated for local use (RFC 1918 addresses). It is a wired network that connects devices within a limited area.
- **WLAN**: Similar to a LAN but uses wireless communication, allowing devices to connect without cables. The main difference is the medium of connection, with WLAN introducing wireless data transmission.

### VPN (Virtual Private Network)

VPNs create secure connections over potentially unsecured networks (like the Internet) to make remote devices appear as if they are part of a local network. There are three main types of VPNs:

1. **Site-To-Site VPN**
   - **Description**: Connects entire networks together, such as branch offices of a company over the Internet, making them function as if they are part of the same local network.
   - **Devices**: Typically involves network devices like routers or firewalls.

2. **Remote Access VPN**
   - **Description**: Allows individual users to connect securely to a network from a remote location. It creates a virtual interface on the client’s device that acts as if it is part of the network.
   - **Example**: Hack The Box uses OpenVPN to create a TUN Adapter for accessing labs.
   - **Split-Tunnel VPN**: Routes only specific network traffic through the VPN, leaving other internet traffic unaffected. This is useful for accessing lab environments while preserving normal internet usage. However, it can be less secure for corporate environments due to potential malware risks.

3. **SSL VPN**
   - **Description**: Operates within a web browser and is increasingly common. It streams applications or entire desktop sessions to the browser.
   - **Example**: Hack The Box Pwnbox is accessed through an SSL VPN.

### Book Terms

These terms provide a broader understanding of network types, although they may not be frequently encountered in everyday practice:

| Network Type                    | Definition                                                   |
|--------------------------------|--------------------------------------------------------------|
| **Global Area Network (GAN)**  | A worldwide network such as the Internet, spanning multiple WANs and connecting global networks via undersea cables or satellites. |
| **Metropolitan Area Network (MAN)** | A broadband network connecting several LANs within a geographical area, such as a city. It provides high-performance connections and can be integrated into WANs and GANs. |
| **Wireless Personal Area Network (WPAN)** | A personal network with a limited range, typically using Bluetooth or Wireless USB. A Bluetooth-based WPAN is known as a Piconet. Used for low-data-rate communication in IoT applications. |

### PAN / WPAN (Personal Area Network / Wireless Personal Area Network)

- **PAN**: A wired network that connects devices over a short range, usually within a single room.
- **WPAN**: A wireless version of a PAN, utilizing technologies like Bluetooth or Wireless USB. It is suitable for connecting devices within a few meters and is used in IoT applications for smart home and automation tasks.

By understanding these network types and their characteristics, you can better manage network infrastructure and address specific needs related to connectivity and security.



# MISC: CDN vs Proxy vs Load Balancer vs POP

## What is a CDN (Content Delivery Network)?

A CDN is a system of distributed servers that delivers web content (like images and videos) to users based on their geographic location. It speeds up access to content and reduces the load on the origin server by caching copies of the content at multiple locations.

## What is a Proxy?

A proxy server acts as an intermediary between a client and the internet. It can cache content to improve load times, filter requests to enhance security, and provide anonymity for users by masking their IP addresses.

## What is a Load Balancer?

A load balancer distributes incoming network or application traffic across multiple servers to ensure no single server becomes overwhelmed. This improves performance, reliability, and helps in managing high volumes of traffic by balancing the load.

## What is a POP (Point of Presence)?

A POP is a physical location where a network connects to other networks or where a CDN has servers to deliver content. It helps reduce latency and improve performance by bringing content closer to users.



# Network Topologies

## Point-to-Point

The simplest network topology with a dedicated connection between two hosts is the point-to-point topology. In this topology, a direct and straightforward physical link exists only between two hosts. These two devices can use these connections for mutual communication.

**Note:** Point-to-point topologies are the basic model of traditional telephony and must not be confused with P2P (Peer-to-Peer architecture).

![Point-To-Point Topology](assets/3-point-to-point.png)

## Bus

All hosts are connected via a transmission medium in the bus topology. Every host has access to the transmission medium and the signals that are transmitted over it. There is no central network component that controls the processes on it. The transmission medium for this can be, for example, a coaxial cable.

Since the medium is shared with all the others, only one host can send, and all the others can only receive and evaluate the data and see whether it is intended for itself.

![Bus Topology](assets/4-but-topology.png)

## Star

The star topology is a network component that maintains a connection to all hosts. Each host is connected to the central network component via a separate link. This is usually a router, a hub, or a switch. These handle the forwarding function for the data packets. To do this, the data packets are received and forwarded to the destination. The data traffic on the central network component can be very high since all data and connections go through it.

![Star Topology](assets/5-star-topology.png)

## Ring

The physical ring topology is such that each host or node is connected to the ring with two cables:

- One for the incoming signals
- Another for the outgoing ones

This means that one cable arrives at each host and one cable leaves. The ring topology typically does not require an active network component. The control and access to the transmission medium are regulated by a protocol to which all stations adhere.

A logical ring topology is based on a physical star topology, where a distributor at the node simulates the ring by forwarding from one port to the next.

The information is transmitted in a predetermined transmission direction. Typically, the transmission medium is accessed sequentially from station to station using a retrieval system from the central station or a token. A token is a bit pattern that continually passes through a ring network in one direction, which works according to the claim token process.

![Ring Topology](assets/6-rign-topology.png)

## Mesh

Many nodes decide about the connections on a physical level and the routing on a logical level in meshed networks. Therefore, meshed structures have no fixed topology. There are two basic structures from the basic concept: the fully meshed and the partially meshed structure.

### Fully Meshed

Each host is connected to every other host in the network in a fully meshed structure. This means that the hosts are meshed with each other. This technique is primarily used in WAN or MAN to ensure high reliability and bandwidth.

In this setup, important network nodes such as routers could be networked together. If one router fails, the others can continue to work without problems, and the network can absorb the failure due to the many connections.

Each node of a fully meshed topology has the same routing functions and knows the neighboring nodes it can communicate with proximity to the network gateway and traffic loads.

### Partially Meshed

In the partially meshed structure, the endpoints are connected by only one connection. In this type of network topology, specific nodes are connected to exactly one other node, and some other nodes are connected to two or more other nodes with a point-to-point connection.

![Mesh Topology](assets/7-mesh-topology.png)

## Tree

The tree topology is an extended star topology that more extensive local networks have in this structure. This is especially useful when several topologies are combined. This topology is often used, for example, in larger company buildings.

There are both logical tree structures according to the spanning tree and physical ones. Modular modern networks, based on structured cabling with a hub hierarchy, also have a tree structure. Tree topologies are also used for broadband networks and city networks (MAN).

![Tree Topology](assets/8-tree-topology.png)

## Hybrid

Hybrid networks combine two or more topologies so that the resulting network does not present any standard topologies. For example, a tree network can represent a hybrid topology in which star networks are connected via interconnected bus networks. However, a tree network that is linked to another tree network is still topologically a tree network. A hybrid topology is always created when two different basic network topologies are interconnected.

![Hybrid Topology](assets/9-hybrid-topology.png)

## Daisy Chain

In the daisy chain topology, multiple hosts are connected by placing a cable from one node to another.

Since this creates a chain of connections, it is also known as a daisy-chain configuration in which multiple hardware components are connected in a series. This type of networking is often found in automation technology (CAN).

Daisy chaining is based on the physical arrangement of the nodes, in contrast to token procedures, which are structural but can be made independent of the physical layout. The signal is sent to and from a component via its previous nodes to the computer system.

![Daisy Chain Topology](assets/10-daisy-chain.png)



# Proxies

Many people have different opinions on what a proxy is:

- **Security Professionals** might think of HTTP Proxies (e.g., BurpSuite) or pivoting with SOCKS/SSH Proxies (e.g., Chisel, ptunnel, sshuttle).
- **Web Developers** use proxies like Cloudflare or ModSecurity to block malicious traffic.
- **Average Users** might see a proxy as a tool to obfuscate their location and access content from another country.
- **Law Enforcement** might associate proxies with illegal activities.

Not all these examples are correct. A proxy is a device or service that sits in the middle of a connection and acts as a mediator. This means the proxy must be able to inspect the contents of the traffic. Without this mediation capability, the device is a gateway, not a proxy.

For many average users, the concept of a proxy is often confused with a VPN, which is technically not a proxy. Most people mistakenly believe that any change in IP address is due to a proxy. While this is a common and harmless misconception, correcting it might lead to tangential discussions.

Proxies generally operate at Layer 7 of the OSI Model. There are several types of proxy services, including:

## Dedicated Proxy / Forward Proxy

A Forward Proxy is what most people imagine when they think of a proxy. In this setup, a client makes a request to a computer (the proxy), which then carries out the request on behalf of the client.

### Examples and Use Cases:

- **Corporate Networks:** Sensitive computers may use a forward proxy to access the Internet, which helps in defense against malware. The proxy needs to be aware of proxy settings to be effective. For instance, malware targeting Firefox might have a harder time if it needs to pull proxy settings specifically for Firefox.
- **Burp Suite:** Often used to forward HTTP requests, but it can also be configured as a reverse or transparent proxy.

![Forward Proxy](assets/11-forward-proxy.png)

## Reverse Proxy

A Reverse Proxy works in the opposite direction of a Forward Proxy. Instead of filtering outgoing requests, it filters incoming ones. Its main goal is to handle requests on behalf of a server.

### Examples and Use Cases:

- **Cloudflare:** Used to protect against DDoS attacks and to filter traffic sent to web servers.
- **Penetration Testing:** Attackers may configure reverse proxies on infected endpoints to bypass firewalls or evade logging. This can help in evading IDS (Intrusion Detection Systems) by tunneling requests through an SSH connection.
- **ModSecurity:** A Web Application Firewall (WAF) that inspects and blocks malicious web requests.

![Reverse Proxy](assets/12-reverse-proxy.png)

## (Non-) Transparent Proxy

Proxies can be either transparent or non-transparent.

### Transparent Proxy

- **Definition:** The client is unaware of its existence. It intercepts and substitutes the client's communication requests.
- **Functionality:** Acts as a communication partner to the outside world.

### Non-Transparent Proxy

- **Definition:** Requires explicit configuration to inform the user and the software of its existence. If not configured, communication to the Internet is blocked since the proxy is the sole communication path.



# Networking Models

Two primary networking models describe the communication and transfer of data from one host to another: the ISO/OSI model and the TCP/IP model. These models offer a structured representation of how data is transferred and managed in a network.

![Networking Models](assets/13-Networking-models.png)

## The OSI Model

The OSI model, often referred to as the ISO/OSI layer model, is a reference model used to describe and define communication between systems. It consists of seven individual layers, each with specific functions.

- **OSI** stands for Open Systems Interconnection model, published by the International Telecommunication Union (ITU) and the International Organization for Standardization (ISO). Hence, it is commonly referred to as the ISO/OSI layer model.

## The TCP/IP Model

TCP/IP (Transmission Control Protocol/Internet Protocol) is a term for a suite of network protocols responsible for the switching and transport of data packets on the Internet. TCP/IP forms the foundation of the Internet and includes more than just TCP and IP. Other protocols in this suite include:

- **ICMP** (Internet Control Message Protocol)
- **UDP** (User Datagram Protocol)

The TCP/IP model provides the necessary functions for data packet transport and switching in both private and public networks.

## ISO/OSI vs. TCP/IP

- **TCP/IP:** Focuses on practical aspects of data transfer, allowing flexibility in how protocols are implemented. It provides a set of guidelines but is less strict than OSI.

- **OSI:** Serves as a comprehensive reference model that is often considered more rigorous. It is used to understand and describe network interactions at a detailed level.

## Packet Transfers

In a layered network system, each layer exchanges data in a format known as a Protocol Data Unit (PDU). For example, when you browse a website:

1. **Data Request:** The remote server software processes the requested data and passes it to the application layer.
2. **Layer Processing:** The data is processed layer by layer, each layer performing its assigned functions.
3. **Transmission:** Data is transferred through the network's physical layer until it reaches the destination server or device.
4. **Reprocessing:** The data is routed through the layers again at the receiving end, where each layer performs its operations.
5. **Usage:** The application layer at the receiving end uses the data.

During this process, each layer adds a header to the PDU from the upper layer. This process, known as encapsulation, helps in controlling and identifying the packet. The headers and data together form the PDU for the next layer. The receiver reverses this process, extracting and using the data.

![Packet Transfers](assets/14-packet-transfers.png)

## Application in Penetration Testing

Both reference models are valuable for penetration testers:

- **TCP/IP Model:** Helps in understanding how connections are established and managed quickly.
- **OSI Model:** Allows for detailed analysis of network traffic, layer by layer.

Penetration testers use these models to intercept and analyze network traffic, gaining insights into data transmission processes and vulnerabilities. Familiarity with both models is crucial for effective network traffic analysis.

![Network Traffic Analysis](assets/15-packet-transfer.png)


# The OSI Model

The goal of defining the ISO/OSI standard was to create a reference model that enables communication between different technical systems across various devices and technologies, ensuring compatibility. The OSI model utilizes seven different layers, each hierarchically based on one another, to achieve this goal. These layers represent the phases through which packets pass in the establishment of a connection, providing a visual representation of how a connection is structured and established.

## OSI Layers

| Layer | Function |
|-------|----------|
| **7. Application** | Controls the input and output of data and provides application functions. |
| **6. Presentation** | Transforms data into a form that is independent of the application, handling system-dependent data representation. |
| **5. Session** | Manages the logical connection between systems, ensuring reliable communication and handling connection breakdowns or other issues. |
| **4. Transport** | Provides end-to-end control of data, including congestion control and segmentation of data streams. |
| **3. Network** | Establishes connections in circuit-switched networks and forwards data packets in packet-switched networks. Responsible for end-to-end transmission from sender to receiver. |
| **2. Data Link** | Ensures reliable and error-free transmission on the medium by dividing bitstreams from layer 1 into blocks or frames. |
| **1. Physical** | Handles the transmission of data via electrical signals, optical signals, or electromagnetic waves on wired or wireless transmission lines. |

## Layer Functions

- **Layers 2-4** are transport-oriented, focusing on reliable data transmission and control.
- **Layers 5-7** are application-oriented, handling data representation and communication protocols.

Each layer performs precisely defined tasks and provides services to the layer directly above it, using the services of the layer below it.

## Data Transmission Process

When an application sends a packet to another system, the layers work as follows:

1. **Sender System:**
   - Data is processed from Layer 7 down to Layer 1.
   
2. **Receiver System:**
   - The received packet is processed from Layer 1 up to Layer 7.

This ensures that both the sender and receiver systems can handle and interpret the data correctly, maintaining the communication's security, reliability, and performance.


# Network Layer

The network layer (<code style="color: cyan">Layer 3</code>) of OSI controls the exchange of data packets, as these cannot be directly routed to the receiver and therefore have to be provided with routing nodes. The data packets are then transferred from node to node until they reach their target. To implement this, the network layer identifies the individual network nodes, sets up and clears connection channels, and takes care of routing and data flow control. When sending the packets, addresses are evaluated, and the data is routed through the network from node to node. There is usually no processing of the data in the layers above the <code style="color: cyan">Layer 3</code> in the nodes. Based on the addresses, the routing and the construction of routing tables are done.

In short, it is responsible for the following functions:

- <code style="color: cyan">Logical Addressing</code>
- <code style="color: cyan">Routing</code>

Protocols are defined in each layer of OSI, and these protocols represent a collection of rules for communication in the respective layer. They are transparent to the protocols of the layers above or below. Some protocols fulfill tasks of several layers and extend over two or more layers. The most used protocols on this layer are:

- <code style="color: aquamarine">IPv4 / IPv6</code>
- <code style="color: aquamarine">IPsec</code>
- <code style="color: aquamarine">ICMP</code>
- <code style="color: aquamarine">IGMP</code>
- <code style="color: aquamarine">RIP</code>
- <code style="color: aquamarine">OSPF</code>

It ensures the routing of packets from source to destination within or outside a subnet. These two subnets may have different addressing schemes or incompatible addressing types. In both cases, the data transmission in each case goes through the entire communication network and includes routing between the network nodes. Since direct communication between the sender and the receiver is not always possible due to the different subnets, packets must be forwarded from nodes (routers) that are on the way. Forwarded packets do not reach the higher layers but are assigned a new intermediate destination and sent to the next node.




# IP Addresses

Each host in the network is identified by the so-called <code style="color: greenyellow">Media Access Control address (MAC)</code>. This allows data exchange within a single network. If the remote host is located in another network, knowledge of the MAC address alone is not enough to establish a connection. Addressing on the Internet is done via the <code style="color: greenyellow">IPv4</code> and/or <code style="color: greenyellow">IPv6</code> address, which is made up of the network address and the host address.

It does not matter whether it is a smaller network, such as a home computer network, or the entire Internet. The IP address ensures the delivery of data to the correct receiver. We can imagine the representation of MAC and IPv4 / IPv6 addresses as follows:

- <code style="color: greenyellow">IPv4 / IPv6</code> - describes the unique postal address and district of the receiver's building.
- <code style="color: greenyellow">MAC</code> - describes the exact floor and apartment of the receiver.

It is possible for a single IP address to address multiple receivers (broadcasting) or for a device to respond to multiple IP addresses. However, it must be ensured that each IP address is assigned only once within the network.

## IPv4 Structure

The most common method of assigning IP addresses is <code style="color: greenyellow">IPv4</code>, which consists of a 32-bit binary number combined into 4 bytes consisting of 8-bit groups (octets) ranging from 0-255. These are converted into more easily readable decimal numbers, separated by dots and represented as dotted-decimal notation.

Thus an IPv4 address can look like this:

**Notation:**
- Binary: `0111 1111.0000 0000.0000 0000.0000 0001`
- Decimal: `127.0.0.1`

Each network interface (network cards, network printers, or routers) is assigned a unique IP address.

The <code style="color: greenyellow">IPv4</code> format allows 4,294,967,296 unique addresses. The IP address is divided into a host part and a network part. The router assigns the host part of the IP address at home or by an administrator. The respective network administrator assigns the network part. On the Internet, this is IANA, which allocates and manages the unique IPs.

In the past, further classification took place here. The IP network blocks were divided into classes A - E. The different classes differed in the host and network shares' respective lengths.

**Class** | **Network Address** | **First Address** | **Last Address** | **Subnetmask** | **CIDR** | **Subnets** | **IPs**
---------|---------------------|-------------------|------------------|----------------|----------|-------------|--------
A        | 1.0.0.0             | 1.0.0.1           | 127.255.255.255  | 255.0.0.0      | /8       | 127         | 16,777,214 + 2
B        | 128.0.0.0           | 128.0.0.1         | 191.255.255.255  | 255.255.0.0    | /16      | 16,384      | 65,534 + 2
C        | 192.0.0.0           | 192.0.0.1         | 223.255.255.255  | 255.255.255.0  | /24      | 2,097,152   | 254 + 2
D        | 224.0.0.0           | 224.0.0.1         | 239.255.255.255  | Multicast      | Multicast| Multicast   | Multicast
E        | 240.0.0.0           | 240.0.0.1         | 255.255.255.255  | Reserved       | Reserved | Reserved    | Reserved

## Subnet Mask

A further separation of these classes into small networks is done with the help of <code style="color: greenyellow">subnetting</code>. This separation is done using the netmasks, which is as long as an IPv4 address. As with classes, it describes which bit positions within the IP address act as network part or host part.

**Class** | **Network Address** | **First Address** | **Last Address** | **Subnetmask** | **CIDR** | **Subnets** | **IPs**
---------|---------------------|-------------------|------------------|----------------|----------|-------------|--------
A        | 1.0.0.0             | 1.0.0.1           | 127.255.255.255  | 255.0.0.0      | /8       | 127         | 16,777,214 + 2
B        | 128.0.0.0           | 128.0.0.1         | 191.255.255.255  | 255.255.0.0    | /16      | 16,384      | 65,534 + 2
C        | 192.0.0.0           | 192.0.0.1         | 223.255.255.255  | 255.255.255.0  | /24      | 2,097,152   | 254 + 2
D        | 224.0.0.0           | 224.0.0.1         | 239.255.255.255  | Multicast      | Multicast| Multicast   | Multicast
E        | 240.0.0.0           | 240.0.0.1         | 255.255.255.255  | Reserved       | Reserved | Reserved    | Reserved

## Network and Gateway Addresses

The two additional IPs added in the IPs column are reserved for the so-called <code style="color: greenyellow">network address</code> and the <code style="color: greenyellow">broadcast address</code>. Another important role plays the <code style="color: greenyellow">default gateway</code>, which is the name for the IPv4 address of the router that couples networks and systems with different protocols and manages addresses and transmission methods. It is common for the default gateway to be assigned the first or last assignable IPv4 address in a subnet. This is not a technical requirement but has become a de-facto standard in network environments of all sizes.

**Class** | **Network Address** | **First Address** | **Last Address** | **Subnetmask** | **CIDR** | **Subnets** | **IPs**
---------|---------------------|-------------------|------------------|----------------|----------|-------------|--------
A        | 1.0.0.0             | 1.0.0.1           | 127.255.255.255  | 255.0.0.0      | /8       | 127         | 16,777,214 + 2
B        | 128.0.0.0           | 128.0.0.1         | 191.255.255.255  | 255.255.0.0    | /16      | 16,384      | 65,534 + 2
C        | 192.0.0.0           | 192.0.0.1         | 223.255.255.255  | 255.255.255.0  | /24      | 2,097,152   | 254 + 2
D        | 224.0.0.0           | 224.0.0.1         | 239.255.255.255  | Multicast      | Multicast| Multicast   | Multicast
E        | 240.0.0.0           | 240.0.0.1         | 255.255.255.255  | Reserved       | Reserved | Reserved    | Reserved

## Broadcast Address

The <code style="color: greenyellow">broadcast IP address</code>'s task is to connect all devices in a network with each other. Broadcast in a network is a message that is transmitted to all participants of a network and does not require any response. In this way, a host sends a data packet to all other participants of the network simultaneously and, in doing so, communicates its IP address, which the receivers can use to contact it. This is the last IPv4 address that is used for the broadcast.

**Class** | **Network Address** | **First Address** | **Last Address** | **Subnetmask** | **CIDR** | **Subnets** | **IPs**
---------|---------------------|-------------------|------------------|----------------|----------|-------------|--------
A        | 1.0.0.0             | 1.0.0.1           | 127.255.255.255  | 255.0.0.0      | /8       | 127         | 16,777,214 + 2
B        | 128.0.0.0           | 128.0.0.1         | 191.255.255.255  | 255.255.0.0    | /16      | 16,384      | 65,534 + 2
C        | 192.0.0.0           | 192.0.0.1         | 223.255.255.255  | 255.255.255.0  | /24      | 2,097,152   | 254 + 2
D        | 224.0.0.0           | 224.0.0.1         | 239.255.255.255  | Multicast      | Multicast| Multicast   | Multicast
E        | 240.0.0.0           | 240.0.0.1         | 255.255.255.255  | Reserved       | Reserved | Reserved    | Reserved

## Binary System

The binary system is a number system that uses only two different states that are represented as two numbers (0 and 1) opposite to the decimal system (0 to 9).

An IPv4 address is divided into 4 octets, as we have already seen. Each octet consists of 8 bits. Each position of a bit in an octet has a specific decimal value. Let's take the following IPv4 address as an example:

**IPv4 Address:** `192.168.10.39`

Here is an example of how the first octet looks like:

**1st Octet - Value:** 192
  IP Addresses
**Values:**         128  64  32  16  8  4  2  1
**Binary:**           1   1   0   0  0  0  0  0

If we calculate the sum of all these values for each octet where the bit is set to 1, we get the sum:

**Octet** | **Values** | **Sum**
---------|------------|--------
1st      | 128 + 64 + 0 + 0 + 0 + 0 + 0 + 0 | = 192
2nd      | 128 + 0 + 32 + 0 + 8 + 0 + 0 + 0   | = 168
3rd      | 0 + 0 + 0 + 0 + 8 + 0 + 2 + 0     | = 10
4th      | 0 + 0 + 32 + 0 + 0 + 4 + 2 + 1     | = 39

The entire representation from binary to decimal would look like this:

**IPv4 - Binary Notation**

  IP Addresses
**Octet:**             1st         2nd         3rd         4th
**Binary:**         1100 0000 . 1010 1000 . 0000 1010 . 0010 0111
**Decimal:**           192    .    168    .     10    .     39

**IPv4 Address:** `192.168.10.39`

This addition takes place for each octet, which results in a decimal representation of the IPv4 address. The subnet mask is calculated in the same way.

**IPv4 - Decimal to Binary**

  IP Addresses
**Values:**         128  64  32  16  8  4  2  1
**Binary:**           1   1   1   1  1  1  1  1

**Octet** | **Values** | **Sum**
---------|------------|--------
1st      | 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 | = 255
2nd      | 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 | = 255
3rd      | 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 | = 255
4th      | 0 + 0 + 0 + 0 + 0 + 0 + 0 + 0     | = 0

**Subnet Mask**

| **Octet** | 1st         | 2nd         | 3rd         | 4th         |
|-----------|-------------|-------------|-------------|-------------|
| **Binary**| 1111 1111   | 1111 1111   | 1111 1111   | 0000 0000   |
| **Decimal**| 255         | 255         | 255         | 0           |


**IPv4 Address:** `192.168.10.39`

**Subnet mask:** `255.255.255.0`

## CIDR

**Classless Inter-Domain Routing (CIDR)** is a method of representation and replaces the fixed assignment between IPv4 address and network classes (A, B, C, D, E). The division is based on the subnet mask or the so-called CIDR suffix, which allows the bitwise division of the IPv4 address space and thus into subnets of any size. The CIDR suffix indicates how many bits from the beginning of the IPv4 address belong to the network. It is a notation that represents the subnet mask by specifying the number of 1-bits in the subnet mask.

Let us stick to the following IPv4 address and subnet mask as an example:

**IPv4 Address:** `192.168.10.39`

**Subnet mask:** `255.255.255.0`

Now the whole representation of the IPv4 address and the subnet mask would look like this:

**CIDR:** `192.168.10.39/24`

The CIDR suffix is, therefore, the sum of all ones in the subnet mask.

**IP Addresses**

| **Octet**  | 1st         | 2nd         | 3rd         | 4th         |
|------------|-------------|-------------|-------------|-------------|
| **Binary** | 1111 1111   | 1111 1111   | 1111 1111   | 0000 0000 (/24) |
| **Decimal**| 255         | 255         | 255         | 0           |


---

# Subnetting

The division of an address range of IPv4 addresses into several smaller address ranges is called subnetting.

A subnet is a logical segment of a network that uses IP addresses with the same network address. We can think of a subnet as a labeled entrance on a large building corridor. For example, this could be a glass door that separates various departments of a company building. With the help of subnetting, we can create a specific subnet by ourselves or find out the following outline of the respective network:

- Network address
- Broadcast address
- First host
- Last host
- Number of hosts

Let us take the following IPv4 address and subnet mask as an example:

- **IPv4 Address:** 192.168.12.160
- **Subnet Mask:** 255.255.255.192
- **CIDR:** 192.168.12.160/26

We already know that an IP address is divided into the network part and the host part.

## Network Part

| Details of   | 1st Octet   | 2nd Octet   | 3rd Octet   | 4th Octet   | Decimal                |
|--------------|--------------|--------------|--------------|--------------|------------------------|
| IPv4         | 1100 0000    | 1010 1000    | 0000 1100    | 1010 0000    | 192.168.12.160/26     |
| Subnet mask  | 1111 1111    | 1111 1111    | 1111 1111    | 1100 0000    | 255.255.255.192       |
| Bits         | /8           | /16          | /24          | /32          |                        |

In subnetting, we use the subnet mask as a template for the IPv4 address. From the 1-bits in the subnet mask, we know which bits in the IPv4 address cannot be changed. These are fixed and therefore determine the "main network" in which the subnet is located.

## Host Part

| Details of   | 1st Octet   | 2nd Octet   | 3rd Octet   | 4th Octet   | Decimal                |
|--------------|--------------|--------------|--------------|--------------|------------------------|
| IPv4         | 1100 0000    | 1010 1000    | 0000 1100    | 1010 0000    | 192.168.12.160/26     |
| Subnet mask  | 1111 1111    | 1111 1111    | 1111 1111    | 1100 0000    | 255.255.255.192       |
| Bits         | /8           | /16          | /24          | /32          |                        |

The bits in the host part can be changed to the first and last address. The first address is the network address, and the last address is the broadcast address for the respective subnet.

The network address is vital for the delivery of a data packet. If the network address is the same for the source and destination address, the data packet is delivered within the same subnet. If the network addresses are different, the data packet must be routed to another subnet via the default gateway.

The subnet mask determines where this separation occurs.

## Separation Of Network & Host Parts

| Details of   | 1st Octet   | 2nd Octet   | 3rd Octet   | 4th Octet   | Decimal                |
|--------------|--------------|--------------|--------------|--------------|------------------------|
| IPv4         | 1100 0000    | 1010 1000    | 0000 1100    | 10|10 0000    | 192.168.12.160/26     |
| Subnet mask  | 1111 1111    | 1111 1111    | 1111 1111    | 11|00 0000    | 255.255.255.192       |
| Bits         | /8           | /16          | /24          | /32          |                        |

### Network Address

So if we now set all bits to 0 in the host part of the IPv4 address, we get the respective subnet's network address.

| Details of   | 1st Octet   | 2nd Octet   | 3rd Octet   | 4th Octet   | Decimal                |
|--------------|--------------|--------------|--------------|--------------|------------------------|
| IPv4         | 1100 0000    | 1010 1000    | 0000 1100    | 10|00 0000    | 192.168.12.128/26     |
| Subnet mask  | 1111 1111    | 1111 1111    | 1111 1111    | 11|00 0000    | 255.255.255.192       |
| Bits         | /8           | /16          | /24          | /32          |                        |

### Broadcast Address

If we set all bits in the host part of the IPv4 address to 1, we get the broadcast address.

| Details of   | 1st Octet   | 2nd Octet   | 3rd Octet   | 4th Octet   | Decimal                |
|--------------|--------------|--------------|--------------|--------------|------------------------|
| IPv4         | 1100 0000    | 1010 1000    | 0000 1100    | 10|11 1111    | 192.168.12.191/26     |
| Subnet mask  | 1111 1111    | 1111 1111    | 1111 1111    | 11|00 0000    | 255.255.255.192       |
| Bits         | /8           | /16          | /24          | /32          |                        |

Since we now know that the IPv4 addresses 192.168.12.128 and 192.168.12.191 are assigned, all other IPv4 addresses are accordingly between 192.168.12.129-190. Now we know that this subnet offers us a total of 64 - 2 (network address & broadcast address) or 62 IPv4 addresses that we can assign to our hosts.

| Hosts             | IPv4               |
|-------------------|-------------------|
| Network Address    | 192.168.12.128    |
| First Host         | 192.168.12.129    |
| Other Hosts        | ...               |
| Last Host          | 192.168.12.190    |
| Broadcast Address   | 192.168.12.191    |

## Subnetting Into Smaller Networks

Let us now assume that we, as administrators, have been given the task of dividing the subnet assigned to us into 4 additional subnets. Thus, it is essential to know that we can only divide the subnets based on the binary system.

| Exponent | Value  |
|----------|--------|
| 2^0     | = 1    |
| 2^1     | = 2    |
| 2^2     | = 4    |
| 2^3     | = 8    |
| 2^4     | = 16   |
| 2^5     | = 32   |
| 2^6     | = 64   |
| 2^7     | = 128  |
| 2^8     | = 256  |

Therefore, we can divide the 64 hosts we know by 4. The 4 is equal to the exponent 2^2 in the binary system, so we find out the number of bits for the subnet mask by which we have to extend it. So we know the following parameters:

- **Subnet:** 192.168.12.128/26
- **Required Subnets:** 4

Now we increase/expand our subnet mask by 2 bits from /26 to /28, and it looks like this:

| Details of   | 1st Octet   | 2nd Octet   | 3rd Octet   | 4th Octet   | Decimal                |
|--------------|--------------|--------------|--------------|--------------|------------------------|
| IPv4         | 1100 0000    | 1010 1000    | 0000 1100    | 1000| 0000    | 192.168.12.128/28     |
| Subnet mask  | 1111 1111    | 1111 1111    | 1111 1111    | 1111| 0000    | 255.255.255.240       |
| Bits         | /8           | /16          | /24          | /32          |                        |

Next, we can divide the 64 IPv4 addresses that are available to us into 4 parts:

| Hosts | Math | Subnets | Host range for each subnet |
|-------|------|---------|---------------------------|
| 64    | /    | 4       | = 16                      |

So we know how big each subnet will be. From now on, we start from the network address given to us (192.168.12.128) and add the 16 hosts 4 times:

| Subnet No. | Network Address      | First Host         | Last Host          | Broadcast Address


# MAC Addresses

Each host in a network has its own 48-bit (6 octets) Media Access Control (MAC) address, represented in hexadecimal format. MAC is the physical address for our network interfaces. There are several different standards for the MAC address:

- **Ethernet (IEEE 802.3)**
- **Bluetooth (IEEE 802.15)**
- **WLAN (IEEE 802.11)**

This is because the MAC address identifies the physical connection (network card, Bluetooth, or WLAN adapter) of a host. Each network card has its individual MAC address, which is configured once on the manufacturer's hardware side but can always be changed, at least temporarily.

## Example of a MAC Address

### MAC Address: 
- `DE:AD:BE:EF:13:37`
- `DE-AD-BE-EF-13-37`
- `DEAD.BEEF.1337`

### Representation
| Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet |
|----------------|------------|------------|------------|------------|------------|------------|
| **Binary**     | 1101 1110  | 1010 1101  | 1011 1110  | 1110 1111  | 0001 0011  | 0011 0111  |
| **Hex**        | DE         | AD         | BE         | EF         | 13         | 37         |



# Understanding MAC Addresses and IP Packet Delivery

When an IP packet is delivered, it must be addressed at Layer 2 (Data Link Layer) to the destination host's physical address (MAC address) or to the router/NAT responsible for routing the packet. Each packet contains a sender's address and a destination address.

## MAC Address Structure

A **MAC address** consists of a total of 6 bytes (48 bits). It is divided into two parts:

1. **Organization Unique Identifier (OUI)**: The first half of the MAC address (3 bytes / 24 bits) is called the OUI. It is defined by the Institute of Electrical and Electronics Engineers (IEEE) and is assigned to manufacturers.
2. **Individual Address Part (NIC)**: The last half (3 bytes) is the unique part assigned by the manufacturer to identify individual devices (Network Interface Controllers or NICs).

### Example MAC Address Representation

For the following MAC address:


- **OUI** (first half of the address): `DE:AD:BE`
- **NIC** (second half of the address): `EF:13:37`

#### OUI Part

| 1st Octet  | 2nd Octet  | 3rd Octet  |
|------------|------------|------------|
| **Binary** | 1101 1110  | 1010 1101  | 1011 1110  |
| **Hex**    | DE         | AD         | BE         |

#### NIC Part

| 4th Octet  | 5th Octet  | 6th Octet  |
|------------|------------|------------|
| **Binary** | 1110 1111  | 0001 0011  | 0011 0111  |
| **Hex**    | EF         | 13         | 37         |

### How MAC Addresses are Used in IP Packet Delivery

- **Same Subnet**: If the target IP address is within the **same subnet**, the IP packet is delivered directly to the destination host’s MAC address.
- **Different Subnet**: If the target IP address is in a **different subnet**, the packet is sent to the MAC address of the **default gateway** (router). The router then forwards the packet to the next destination.

### Role of ARP (Address Resolution Protocol)

To map an IP address to a MAC address, IPv4 networks use the **Address Resolution Protocol (ARP)**. ARP is used to discover the MAC address associated with a given IP address within a local network.

### Reserved MAC Addresses

Similar to IP addresses, certain MAC address ranges are reserved for specific purposes, such as the **local range** for private use.

---

For more information on MAC addresses and how they function in networking, refer to [IEEE MAC Address Standards](https://standards.ieee.org/products-services/regauth/oui/index.html).


# MAC Addresses Explained

Every device on a network has a unique **48-bit Media Access Control (MAC)** address, typically represented in hexadecimal format. Different standards include:

- **Ethernet (IEEE 802.3)**
- **Bluetooth (IEEE 802.15)**
- **WLAN (IEEE 802.11)**

Each network card comes with its own MAC address, which can be changed temporarily.

## Example of a MAC Address
- **Format**: DE:AD:BE:EF:13:37
- **Hexadecimal**: DE AD BE EF 13 37
- **Binary**: 11011110 10101101 10111110 11101111 00010011 00110111

### Structure of MAC Address
- **Organization Unique Identifier (OUI)**: First 3 bytes (assigned to manufacturers).
- **Individual Address**: Last 3 bytes (assigned by the manufacturer).

## How MAC Addresses Work
When sending data:
- **Same Network**: Data goes directly to the target's MAC address.
- **Different Network**: Data is sent to the MAC address of a router (default gateway).

The **Address Resolution Protocol (ARP)** helps devices find each other by converting IP addresses to MAC addresses.

## Types of MAC Addresses
1. **Unicast**: One-to-one communication (last bit is 0).
2. **Multicast**: One-to-many communication (last bit is 1).
3. **Broadcast**: Sent to all devices (all bits are 1, e.g., FF:FF:FF:FF:FF:FF).

### Address Types
- **Globally Unique**: Assigned by IEEE.
- **Locally Administered**: Can be changed by network administrators.

## Security Risks with MAC Addresses
MAC addresses can be manipulated, leading to security issues:
- **MAC Spoofing**: Changing a device's MAC address to impersonate another.
- **MAC Flooding**: Overloading a switch with fake MAC addresses.

## ARP (Address Resolution Protocol)
ARP helps devices find MAC addresses based on IP addresses:
1. **ARP Request**: A device asks, "Who has this IP address?"
2. **ARP Reply**: The device with that IP responds with its MAC address.

## ARP Spoofing
An attack where false ARP messages link your MAC address with another device’s IP, allowing traffic interception. To protect against ARP spoofing, use secure protocols and implement network security measures like firewalls and intrusion detection systems.



# IPv6 Addresses

**IPv6** is the successor to **IPv4**, featuring a 128-bit address length. It allows for a much larger address space and provides several advantages over IPv4, such as:

- **Larger Address Space**: Approximately 340 undecillion addresses.
- **Address Self-Configuration**: Through Stateless Address Autoconfiguration (SLAAC).
- **Multiple Addresses**: Each interface can have multiple IPv6 addresses.
- **Faster Routing**: More efficient data handling.
- **Mandatory IPsec**: For end-to-end encryption.
- **Larger Data Packages**: Supports packets up to 4 GBytes.

### Key Differences Between IPv4 and IPv6

| Feature                 | IPv4             | IPv6             |
|-------------------------|------------------|------------------|
| Bit Length              | 32-bit           | 128-bit          |
| OSI Layer               | Network Layer     | Network Layer     |
| Addressing Range        | ~4.3 billion     | ~340 undecillion  |
| Representation          | Decimal          | Hexadecimal       |
| Prefix Notation         | 10.10.10.0/24    | fe80::dd80:b1a9:6687:2d3b/64 |
| Dynamic Addressing      | DHCP             | SLAAC / DHCPv6    |
| IPsec                   | Optional         | Mandatory         |

### Types of IPv6 Addresses

1. **Unicast**: For a single interface.
2. **Anycast**: For multiple interfaces; only one receives the packet.
3. **Multicast**: For multiple interfaces; all receive the same packet.
4. **Broadcast**: Does not exist; realized using multicast.

### Hexadecimal System

The hexadecimal system is used to make binary representations more readable. It represents 16 states (0-F) with a single character.

| Decimal | Hex | Binary     |
|---------|-----|------------|
| 1       | 1   | 0001       |
| 2       | 2   | 0010       |
| 3       | 3   | 0011       |
| 4       | 4   | 0100       |
| 5       | 5   | 0101       |
| 6       | 6   | 0110       |
| 7       | 7   | 0111       |
| 8       | 8   | 1000       |
| 9       | 9   | 1001       |
| 10      | A   | 1010       |
| 11      | B   | 1011       |
| 12      | C   | 1100       |
| 13      | D   | 1101       |
| 14      | E   | 1110       |
| 15      | F   | 1111       |

#### Example: IPv4 to Hexadecimal
For the IPv4 address **192.168.12.160**:
- **Binary**: 11000000 10101000 00001100 10100000
- **Hex**: C0 A8 0C A0

### IPv6 Address Representation

An IPv6 address consists of 16 bytes, represented in hexadecimal notation as eight blocks of 16 bits (four hex digits), separated by colons. Leading zeros can be omitted, and consecutive blocks of zeros can be shortened with double colons (::), but this can only be done once.

#### Example IPv6 Address
- **Full IPv6**: fe80:0000:0000:0000:dd80:b1a9:6687:2d3b/64
- **Short IPv6**: fe80::dd80:b1a9:6687:2d3b/64

### Parts of an IPv6 Address
1. **Network Prefix**: Identifies the network, subnet, or address range.
2. **Interface Identifier**: Formed from the 48-bit MAC address, converted to a 64-bit address.

The default prefix length is **/64**, with common prefixes including **/32**, **/48**, and **/56**.

### RFC 5952 Notation Rules
- All alphabetical characters are in lower case.
- Leading zeros of a block are omitted.
- Consecutive blocks of four zeros are shortened with two colons (::), done only once.


# Networking Key Terminology

In the vast field of information technology, understanding key terminology is essential. This guide provides an overview of some of the most common protocols and their descriptions. Note that this list is not exhaustive, and it's beneficial to review and expand it as you learn.

| Protocol                                          | Acronym | Description                                                                                          |
|---------------------------------------------------|---------|------------------------------------------------------------------------------------------------------|
| Wired Equivalent Privacy                          | WEP     | A security protocol commonly used to secure wireless networks.                                      |
| Secure Shell                                      | SSH     | A secure network protocol for logging into and executing commands on remote systems.                |
| File Transfer Protocol                             | FTP     | A protocol used to transfer files between systems.                                                  |
| Simple Mail Transfer Protocol                      | SMTP    | A protocol for sending and receiving emails.                                                       |
| Hypertext Transfer Protocol                        | HTTP    | A client-server protocol for transmitting data over the internet.                                   |
| Server Message Block                               | SMB     | A protocol for sharing files, printers, and other resources in a network.                          |
| Network File System                                | NFS     | A protocol for accessing files over a network.                                                     |
| Simple Network Management Protocol                 | SNMP    | A protocol for managing network devices.                                                            |
| Wi-Fi Protected Access                             | WPA     | A wireless security protocol that protects networks from unauthorized access.                       |
| Temporal Key Integrity Protocol                    | TKIP    | A security protocol for wireless networks, considered less secure than WPA.                        |
| Network Time Protocol                              | NTP     | A protocol for synchronizing the clocks of computers on a network.                                 |
| Virtual Local Area Network                         | VLAN    | A method for segmenting a network into multiple logical networks.                                   |
| VLAN Trunking Protocol                             | VTP     | A Layer 2 protocol used to maintain a virtual LAN spanning multiple switches.                       |
| Routing Information Protocol                       | RIP     | A distance-vector routing protocol used in LANs and WANs.                                          |
| Open Shortest Path First                           | OSPF    | An interior gateway protocol for routing within a single Autonomous System (AS).                    |
| Interior Gateway Routing Protocol                  | IGRP    | A Cisco proprietary protocol designed for routing within autonomous systems.                        |
| Enhanced Interior Gateway Routing Protocol         | EIGRP   | An advanced distance-vector routing protocol for routing IP traffic within a network.               |
| Pretty Good Privacy                                | PGP     | An encryption program used for securing emails, files, and other data.                             |
| Network News Transfer Protocol                     | NNTP    | A protocol for distributing and retrieving messages in newsgroups.                                  |
| Cisco Discovery Protocol                           | CDP     | A proprietary protocol by Cisco for discovering and managing Cisco devices in a network.            |
| Hot Standby Router Protocol                        | HSRP    | A protocol for providing redundancy in Cisco routers in case of device failure.                     |
| Virtual Router Redundancy Protocol                 | VRRP    | A protocol for automatically assigning available IP routers to participating hosts.                 |
| Spanning Tree Protocol                             | STP     | A network protocol for ensuring a loop-free topology in Layer 2 Ethernet networks.                  |
| Terminal Access Controller Access-Control System   | TACACS  | A protocol providing centralized authentication, authorization, and accounting for network access.  |
| Session Initiation Protocol                        | SIP     | A signaling protocol for establishing and terminating real-time sessions over an IP network.        |
| Voice Over IP                                     | VOIP    | Technology allowing telephone calls to be made over the internet.                                   |
| Extensible Authentication Protocol                 | EAP     | A framework for authentication supporting multiple methods like passwords and digital certificates. |
| Lightweight Extensible Authentication Protocol      | LEAP    | A proprietary wireless authentication protocol by Cisco, based on EAP.                             |
| Protected Extensible Authentication Protocol       | PEAP    | A security protocol providing an encrypted tunnel for wireless and other networks.                  |
| Systems Management Server                          | SMS     | A solution for managing networks, systems, and mobile devices.                                      |
| Microsoft Baseline Security Analyzer               | MBSA    | A free tool from Microsoft for detecting potential security vulnerabilities in systems.             |
| Supervisory Control and Data Acquisition           | SCADA   | An industrial control system for monitoring and controlling industrial processes.                    |
| Virtual Private Network                            | VPN     | Technology allowing users to create a secure, encrypted connection over the internet.               |
| Internet Protocol Security                         | IPsec   | A protocol for secure communication over a network, commonly used in VPNs.                          |
| Point-to-Point Tunneling Protocol                  | PPTP    | A protocol for creating a secure, encrypted tunnel for remote access.                              |
| Network Address Translation                        | NAT     | A technology allowing multiple devices on a private network to connect to the internet via one public IP address. |
| Carriage Return Line Feed                          | CRLF    | Combines control characters to indicate the end of a line in certain text file formats.            |
| Asynchronous JavaScript and XML                   | AJAX    | A web development technique for creating dynamic web pages using JavaScript and XML/JSON.          |
| Internet Server Application Programming Interface   | ISAPI   | Allows creating performance-oriented web extensions for web servers using APIs.                     |
| Uniform Resource Identifier                        | URI     | A syntax used to identify resources on the Internet.                                               |
| Uniform Resource Locator                           | URL     | A subset of URI that identifies web pages or resources, including the protocol and domain name.    |
| Internet Key Exchange                              | IKE     | A protocol for establishing secure connections, used in VPNs for authentication and encryption.     |
| Generic Routing Encapsulation                      | GRE     | A protocol for encapsulating data transmitted within VPN tunnels.                                   |
| Remote Shell                                       | RSH     | A Unix program allowing execution of commands on remote computers.                                  |



# Common Protocols

Internet protocols are standardized rules defined in RFCs (Request for Comments) that specify how devices on a network communicate. They ensure consistent and reliable information exchange, regardless of hardware and software differences. There are two main types of connections used on networks:

- **Transmission Control Protocol (TCP)**: A connection-oriented protocol that establishes a virtual connection before transmitting data.
- **User Datagram Protocol (UDP)**: A connectionless protocol that sends data packets without establishing a prior connection.

## Transmission Control Protocol (TCP)

TCP establishes a connection using a Three-Way Handshake and maintains it until data transfer is complete. This reliability comes with overhead, making it slower than UDP.

### Key TCP Protocols

| Protocol                                   | Acronym | Port      | Description                                                 |
|--------------------------------------------|---------|-----------|-------------------------------------------------------------|
| Telnet                                     | Telnet  | 23        | Remote login service                                        |
| Secure Shell                               | SSH     | 22        | Secure remote login service                                 |
| Simple Network Management Protocol         | SNMP    | 161-162   | Manage network devices                                      |
| Hyper Text Transfer Protocol               | HTTP    | 80        | Used to transfer webpages                                   |
| Hyper Text Transfer Protocol Secure        | HTTPS   | 443       | Used for secure webpage transfer                            |
| Domain Name System                         | DNS     | 53        | Lookup domain names                                        |
| File Transfer Protocol                     | FTP     | 20-21     | Used to transfer files                                      |
| Trivial File Transfer Protocol             | TFTP    | 69        | Used to transfer files                                      |
| Network Time Protocol                      | NTP     | 123       | Synchronize computer clocks                                 |
| Simple Mail Transfer Protocol              | SMTP    | 25        | Used for email transfer                                     |
| Post Office Protocol                       | POP3    | 110       | Used to retrieve emails                                     |
| Internet Message Access Protocol           | IMAP    | 143       | Used to access emails                                       |
| Server Message Block                       | SMB     | 445       | Used to transfer files                                      |
| Network File System                        | NFS     | 111, 2049 | Used to mount remote systems                                |
| Bootstrap Protocol                         | BOOTP   | 67, 68    | Used to bootstrap computers                                  |
| Kerberos                                   | Kerberos| 88        | Used for authentication and authorization                   |
| Lightweight Directory Access Protocol      | LDAP    | 389       | Used for directory services                                  |
| Remote Authentication Dial-In User Service | RADIUS  | 1812, 1813| Used for authentication and authorization                   |
| Dynamic Host Configuration Protocol        | DHCP    | 67, 68    | Used to configure IP addresses                              |
| Remote Desktop Protocol                    | RDP     | 3389      | Used for remote desktop access                              |
| Network News Transfer Protocol             | NNTP    | 119       | Used to access newsgroups                                   |
| Remote Procedure Call                      | RPC     | 135, 137-139| Used to call remote procedures                             |
| Identification Protocol                    | Ident   | 113       | Used to identify user processes                             |
| Internet Control Message Protocol          | ICMP    | 0-255     | Used to troubleshoot network issues                          |
| Internet Group Management Protocol         | IGMP    | 0-255     | Used for multicasting                                       |
| Internet Key Exchange                      | IKE     | 500       | Used for secure connection establishment                    |
| Microsoft SQL Server                       | ms-sql-s| 1433      | Used for client connections to Microsoft SQL Server         |
| Point-to-Point Tunneling Protocol         | PPTP    | 1723      | Used to create VPNs                                        |
| Secure Copy Protocol                       | SCP     | 22        | Securely copy files between systems                         |

## User Datagram Protocol (UDP)

UDP is a faster but less reliable protocol that sends data packets without establishing a connection. This makes it suitable for applications where speed is more important than reliability.

### Key UDP Protocols

| Protocol                                   | Acronym | Port      | Description                                                 |
|--------------------------------------------|---------|-----------|-------------------------------------------------------------|
| Domain Name System                         | DNS     | 53        | Resolves domain names to IP addresses                       |
| Trivial File Transfer Protocol             | TFTP    | 69        | Used to transfer files                                      |
| Network Time Protocol                      | NTP     | 123       | Synchronizes clocks in a network                            |
| Simple Network Management Protocol         | SNMP    | 161       | Monitors and manages network devices                        |
| Routing Information Protocol               | RIP     | 520       | Exchanges routing information between routers                |
| Dynamic Host Configuration Protocol        | DHCP    | 67       | Assigns IP addresses dynamically                            |
| Telnet                                     | TELNET  | 23        | Text-based remote access communication                      |
| Internet Protocol Security                 | IPsec   | 500       | Provides secure communication, commonly used in VPNs       |

## Internet Control Message Protocol (ICMP)

ICMP is used for error reporting and status information. It facilitates communication between devices for various purposes, including connectivity tests (e.g., ping requests).

### ICMP Requests and Messages

#### Requests
- **Echo Request**: Tests if a device is reachable (ping).
- **Timestamp Request**: Determines the time on a remote device.
- **Address Mask Request**: Requests the subnet mask of a device.

#### Messages
- **Echo Reply**: Response to an echo request.
- **Destination Unreachable**: Indicates a packet cannot reach its destination.
- **Redirect**: Informs a device to send packets to a different router.
- **Time Exceeded**: Sent when a packet takes too long to reach its destination.

### Time-To-Live (TTL)
The TTL field limits a packet's lifetime, preventing indefinite circulation. Each router decrements the TTL value, and when it reaches zero, the packet is discarded, triggering a Time Exceeded message.

## Voice over Internet Protocol (VoIP)

VoIP allows voice and multimedia communications over broadband connections. Common applications include Skype and Zoom. The most used ports for VoIP are:

- **SIP (Session Initiation Protocol)**: TCP/5060 and TCP/5061
- **H.323**: TCP/1720

### Common SIP Methods

| Method    | Description                                     |
|-----------|-------------------------------------------------|
| INVITE    | Initiates a session or invites another endpoint.|
| ACK       | Confirms receipt of an INVITE request.         |
| BYE       | Terminates a session.                          |
| CANCEL    | Cancels a pending INVITE request.              |
| REGISTER  | Registers a SIP user agent with a SIP server.  |
| OPTIONS   | Requests information about SIP server capabilities.|

### Information Disclosure
SIP can expose user information, making it susceptible to attacks. For example, the SIP OPTIONS request can be used to gather details about a server’s capabilities.

This structured overview serves as a foundational reference for understanding common networking protocols. You can expand or modify it further as needed!


# Wireless Networks: Overview and Security

## What Are Wireless Networks?
Wireless networks allow devices like laptops, smartphones, and tablets to connect and communicate without physical cables. They utilize radio frequency (RF) technology to transmit data. Each device has a wireless adapter that converts data into RF signals, which are sent through the air to other devices on the network.

### Types of Wireless Networks
1. **Wireless Local Area Networks (WLAN)**: Typically used in homes or small offices (e.g., WiFi), with a range of a few hundred feet.
2. **Wireless Wide Area Networks (WWAN)**: Uses cellular technology (3G, 4G, 5G) for broader coverage, such as cities or regions.

To connect, devices must be within range of a wireless access point (WAP) and configured with the correct network settings (SSID and password).

## WiFi Connection Process
Connecting to a WiFi network involves several steps governed by the IEEE 802.11 protocol:
1. **Connection Request**: A device sends a request to the WAP, including its MAC address, SSID, supported data rates, channels, and security protocols.
2. **WAP Response**: The WAP acknowledges the request, allowing the device to connect.
3. **Data Transmission**: Once connected, devices can communicate with each other and access the Internet via the WAP.

## Communication and Signal Strength
WiFi communication typically occurs over the 2.4 GHz or 5 GHz bands. The strength and range of RF signals can be affected by obstacles, interference, and environmental factors. Techniques like spread spectrum transmission and error correction are used to maintain reliable communication.

## Security Features in Wireless Networks
Wireless networks implement various security measures to protect against unauthorized access and ensure data integrity.

### Key Security Features
1. **Encryption**: Protects data transmitted over the network. Common protocols include:
   - **Wired Equivalent Privacy (WEP)**: Outdated and insecure.
   - **WiFi Protected Access (WPA)**: Offers better security.
   - **WPA2/WPA3**: Current standards using advanced encryption methods like AES.

2. **Access Control**: Ensures only authorized devices can connect, typically managed through passwords or MAC address filtering.

3. **Firewalls**: Built-in firewalls on routers monitor and control incoming and outgoing network traffic based on security rules.

## Authentication Protocols
- **Lightweight Extensible Authentication Protocol (LEAP)**: Uses shared keys for authentication.
- **Protected Extensible Authentication Protocol (PEAP)**: Utilizes secure tunneled authentication with digital certificates.

## Conclusion
Wireless networks provide flexibility and convenience in connectivity but require robust security measures to protect data and maintain network integrity. Implementing modern encryption standards and authentication protocols is crucial for safeguarding wireless communications.



# Virtual Private Networks (VPN)

## Overview
A Virtual Private Network (VPN) creates a secure and encrypted connection between a private network and a remote device. This technology allows remote machines to access private network resources securely, providing confidentiality and safe access to internal services.

For example, an administrator can manage internal servers remotely via a VPN, which encrypts the data transfer and assigns a local IP address to the remote device, enabling access to the network.

## Benefits of VPNs
- **Security**: VPNs encrypt the connection, making it difficult for attackers to intercept sensitive information.
- **Remote Access**: Employees can access network resources from anywhere with an internet connection, facilitating remote work.
- **Cost-Effective**: VPNs are generally more affordable than leased lines or dedicated connections, as they utilize the public internet.
- **Interconnectivity**: VPNs can link multiple remote locations (e.g., branch offices) into a single private network, simplifying resource management.

## Requirements for VPN Operation
| Requirement       | Description                                                                                  |
|-------------------|----------------------------------------------------------------------------------------------|
| **VPN Client**    | Installed on the remote device to establish and maintain a VPN connection (e.g., OpenVPN client). |
| **VPN Server**    | Accepts VPN connections from clients and routes traffic to the private network.              |
| **Encryption**     | Utilizes various algorithms (e.g., AES, IPsec) to secure the connection and protect data.  |
| **Authentication** | VPN server and client must authenticate each other using a shared secret, certificate, etc. |

## VPN Connection Process
At the TCP/IP layer, VPN connections often use the Encapsulating Security Payload (ESP) protocol to ensure secure data exchange over the public internet.

## Protocols Used in VPNs

### IPsec
Internet Protocol Security (IPsec) provides encryption and authentication for internet communications by encrypting data payloads and adding authentication headers.

- **Protocols**:
  - **Authentication Header (AH)**: Ensures packet integrity and authenticity without encryption.
  - **Encapsulating Security Payload (ESP)**: Encrypts data payloads and can include authentication.

- **Modes**:
  - **Transport Mode**: Encrypts and authenticates only the data payload, used for end-to-end communications.
  - **Tunnel Mode**: Encrypts the entire IP packet, including the header, commonly used for creating VPN tunnels.

#### Required Protocols for IPsec
| Protocol          | Port     | Description                                                                                   |
|-------------------|----------|-----------------------------------------------------------------------------------------------|
| **Internet Protocol (IP)** | UDP/50-51 | Provides routing for packets between VPN client and server.                                  |
| **Internet Key Exchange (IKE)** | UDP/500   | Establishes secure communication and negotiates shared secret keys for encryption.            |
| **Encapsulating Security Payload (ESP)** | UDP/4500  | Encrypts VPN traffic between client and server using negotiated keys.                          |

### PPTP
Point-to-Point Tunneling Protocol (PPTP) establishes secure tunnels for VPNs but is no longer considered secure due to vulnerabilities. It has been largely replaced by more secure protocols like L2TP/IPsec and OpenVPN.

- **Vulnerabilities**: PPTP's reliance on MSCHAPv2 and outdated DES encryption makes it susceptible to attacks, leading to its decline in use since 2012.

## Conclusion
VPNs are essential for providing secure remote access to private networks. While older protocols like PPTP have fallen out of favor due to security concerns, more robust options like IPsec and OpenVPN are widely recommended for ensuring secure communications over the internet.



# Cisco IOS and VLANs

## Cisco IOS Overview

Cisco IOS is the operating system used in Cisco network devices like routers and switches. It helps manage and operate these devices, offering various features important for modern networks, such as:

- **Support for IPv6**
- **Quality of Service (QoS)**
- **Security features** (like encryption and authentication)
- **Virtualization** (such as VPLS and VRF)

### Management Methods

Cisco IOS can be managed through:

- **Command Line Interface (CLI)**: The most common way to configure devices.
- **Graphical User Interface (GUI)**: A visual way to manage devices.

### Supported Protocols and Services

Cisco IOS supports several protocols and services essential for network operations:

| Protocol Type             | Description                                                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------|
| **Routing Protocols**     | Used to route data packets (e.g., OSPF, BGP).                                               |
| **Switching Protocols**   | Helps manage switches (e.g., VTP, STP).                                                     |
| **Network Services**      | Automates IP address assignments (e.g., DHCP).                                              |
| **Security Features**     | Controls access to resources (e.g., Access Control Lists - ACLs).                           |

### Password Types in Cisco IOS

Different passwords are used for various access levels:

| Password Type       | Description                                                                  |
|---------------------|------------------------------------------------------------------------------|
| **User**            | Used to log in to Cisco IOS.                                               |
| **Enable Password** | Allows entry into "enable" mode for advanced functions.                    |
| **Secret**          | Secures access to certain functions.                                        |
| **Enable Secret**   | An extra-secure password for "enable" mode, stored encrypted.              |

## VLANs: Virtual Local Area Networks

### What is a VLAN?

A VLAN is a way to logically group devices connected to a switch, creating separate broadcast domains. This means a broadcast from one VLAN won’t affect devices in another VLAN.

#### Benefits of VLANs

1. **Better Organization**: Group devices by department or function.
2. **Increased Security**: Prevent unauthorized access to other VLANs.
3. **Simplified Administration**: Manage devices without considering their physical location.
4. **Improved Performance**: Reduces broadcast traffic, freeing up bandwidth.

### VLAN Setup for Departments

Here’s an example of how a network can be segmented by department:

| Department | VLAN ID | Subnet              |
|------------|---------|---------------------|
| Servers    | VLAN 10 | 192.168.1.0/24      |
| C-Level    | VLAN 20 | 192.168.2.0/24      |
| Finance    | VLAN 30 | 192.168.3.0/24      |
| HR         | VLAN 40 | 192.168.4.0/24      |
| Marketing   | VLAN 50 | 192.168.5.0/24      |
| Support    | VLAN 60 | 192.168.6.0/24      |

### VLAN Membership

VLANs can be assigned in two ways:

- **Static VLANs**: Manually assigned to each port. More secure but requires manual configuration.
- **Dynamic VLANs**: Automatically assigned based on MAC addresses. More flexible but less secure.

### Access and Trunk Ports

- **Access Ports**: Carry traffic for only one VLAN. Any traffic coming in is assumed to belong to that VLAN.
- **Trunk Ports**: Can carry traffic for multiple VLANs. Used to connect switches and routers.

### VLAN Identification

Standard Ethernet frames do not include VLAN information, so two main trunking methods are used to keep track of VLAN data:

- **Inter-Switch Link (ISL)**: An older Cisco protocol that is now deprecated.
- **IEEE 802.1Q**: A widely used standard that adds VLAN tags to Ethernet frames.

By understanding these concepts, network administrators can effectively manage VLANs and enhance network performance and security.

![image](https://github.com/user-attachments/assets/bc07c945-f33a-48e7-8eb3-da5cad919159)


# VLAN Tagging and NIC Configuration

## Tag Protocol Identifier (TPID) and Tag Control Information (TCI)

- **TPID**: A 16-bit field always set to `0x8100` that identifies an Ethernet frame as an 802.1Q-tagged frame.
- **TCI**: A 16-bit field containing:
  - **Priority Code Point (PCP)**
  - **Drop Eligible Indicator (DEI)** (previously known as Canonical Format Indicator - CFI)
  - **VLAN Identifier (VID)**: The most important part for VLANs, occupying the low-order 12 bits. 

Since VID is 12 bits, it allows for \(2^{12} - 2 = 4096\) VLAN IDs (0 and 4095 are reserved), which means an 802.1Q-tagged frame can represent up to 4094 VLANs. The practice of inserting multiple 802.1Q tags within a single packet is called **Double Tagging**, introduced by 802.1ad.

### VLAN Tagging and Untagging

- **VLAN Tagging**: Inserting VLAN information into an 802.1Q Ethernet header.
- **VLAN Untagging**: Removing VLAN information from an 802.1Q-tagged Ethernet frame and forwarding the packet to the appropriate destination ports.

## VLAN-Capable NICs

Some network interface cards (NICs) support VLAN tagging, allowing you to assign VLAN IDs to them.

### Assigning a VLAN ID to a NIC in Linux

In Linux, creating a VLAN is done by creating an interface on top of another, called a parent interface. This VLAN interface will tag packets with the assigned VLAN ID, while returning packets will be untagged.

#### Steps to Assign a VLAN ID in Linux

[Read Here](https://academy.hackthebox.com/module/34/section/1878)


# Key Exchange Mechanisms

Key exchange methods are used to securely exchange cryptographic keys between two parties. This is crucial in many cryptographic protocols, as the security of encryption depends on keeping these keys secret. Different key exchange methods have unique characteristics and levels of security, and the choice of method depends on the specific circumstances and requirements of the situation.

## Common Key Exchange Methods

### Diffie-Hellman (DH)

- **Overview**: Allows two parties to agree on a shared secret key without prior communication or shared information.
- **Use**: Commonly used in establishing secure communication channels, such as in the TLS protocol.
- **Limitations**:
  - Vulnerable to Man-in-the-Middle (MITM) attacks, where an attacker intercepts and alters communications.
  - Requires significant CPU power to generate keys, making it less suitable for low-power devices.

### Rivest–Shamir–Adleman (RSA)

- **Overview**: Uses large prime numbers to generate a shared secret key.
- **Security**: Relies on the difficulty of factoring the product of two large primes.
- **Applications**:
  - Encrypting and signing messages for confidentiality and authentication.
  - Protecting data in transit (e.g., SSL/TLS).
  - Generating and verifying digital signatures.
  - Authenticating users and devices (e.g., Kerberos).

### Elliptic Curve Diffie-Hellman (ECDH)

- **Overview**: A variant of Diffie-Hellman that uses elliptic curve cryptography.
- **Advantages**:
  - More efficient and secure than traditional Diffie-Hellman.
  - Supports forward secrecy, ensuring past communications remain secure even if keys are compromised.
  - Used in protocols like TLS and VPNs.

### Elliptic Curve Digital Signature Algorithm (ECDSA)

- **Overview**: Uses elliptic curve cryptography to create digital signatures.
- **Benefits**: Provides enhanced security and efficiency for digital signature generation.

## Internet Key Exchange (IKE)

IKE is a protocol used to establish and maintain secure communication sessions, particularly in VPNs. It combines the Diffie-Hellman algorithm with other cryptographic techniques to securely exchange keys and negotiate security parameters.

### Modes of IKE

- **Main Mode**:
  - Default mode for IKE, considered more secure.
  - Performs key exchange in three phases, providing greater flexibility and security.
  - May result in slower performance due to the number of exchanges.

- **Aggressive Mode**:
  - Provides faster performance by reducing the number of exchanges to two phases.
  - Less secure than main mode as it does not protect identities.

### Pre-Shared Keys (PSK)

A Pre-Shared Key is a secret shared between the two parties before the key exchange. It authenticates the parties and establishes a shared secret for encrypting communications.

- **Advantages**:
  - Adds an extra layer of security by ensuring both parties authenticate each other.
  
- **Limitations**:
  - Securely exchanging the PSK can be challenging.
  - If compromised, it may lead to security breaches, such as MITM attacks.

## Summary of Algorithms

| Algorithm       | Acronym | Security Characteristics                                 |
|------------------|---------|--------------------------------------------------------|
| Diffie-Hellman   | DH      | Relatively secure and computationally efficient        |
| Rivest–Shamir–Adleman | RSA    | Widely used and secure, but computationally intensive  |
| Elliptic Curve Diffie-Hellman | ECDH   | Enhanced security and efficiency compared to DH       |
| Elliptic Curve Digital Signature Algorithm | ECDSA  | Provides enhanced security and efficiency for signatures |

Understanding these key exchange mechanisms is essential for implementing secure communication in various applications and protocols.




# Authentication Protocols

Authentication protocols are essential in networking for verifying the identities of users, devices, and other entities. These protocols provide a secure and standardized way to ensure that only authorized entities can access network resources, helping to prevent unauthorized access and other security threats.

Authentication protocols also facilitate the secure exchange of information, which is vital for maintaining the confidentiality and integrity of sensitive data. Below are some commonly used authentication protocols:

## Common Authentication Protocols

| Protocol | Description |
|----------|-------------|
| **Kerberos** | A Key Distribution Center (KDC) based authentication protocol that uses tickets in domain environments. |
| **SRP** | A password-based authentication protocol that protects against eavesdropping and man-in-the-middle attacks using cryptography. |
| **SSL** | A cryptographic protocol for secure communication over a computer network. |
| **TLS** | The successor to SSL, providing secure communication over the internet. |
| **OAuth** | An open standard for authorization that allows users to grant third-party access to their resources without sharing passwords. |
| **OpenID** | A decentralized authentication protocol allowing users to sign in to multiple websites with a single identity. |
| **SAML** | Security Assertion Markup Language is an XML-based standard for securely exchanging authentication and authorization data. |
| **2FA** | An authentication method using two different factors to verify a user's identity. |
| **FIDO** | The Fast IDentity Online Alliance develops open standards for strong authentication. |
| **PKI** | A system for securely exchanging information based on public and private key cryptography. |
| **SSO** | Single Sign-On allows a user to access multiple applications with a single set of credentials. |
| **MFA** | Multi-Factor Authentication uses multiple factors (knowledge, possession, inherence) to verify identity. |
| **PAP** | A simple protocol that sends passwords in clear text over the network. |
| **CHAP** | Uses a three-way handshake to verify a user's identity. |
| **EAP** | A framework supporting multiple authentication methods, allowing various technologies for user identity verification. |
| **SSH** | A network protocol for secure communication, used for remote access and file transfer, employing encryption for protection. |
| **HTTPS** | A secure version of HTTP that uses SSL/TLS for encryption and authentication. |
| **LEAP** | A Cisco wireless authentication protocol that uses EAP for mutual authentication, vulnerable to dictionary attacks. |
| **PEAP** | A secure tunneling protocol based on EAP, using TLS to encrypt communication and providing robust authentication methods. |

## Comparison of LEAP and PEAP

- **LEAP**: 
  - Developed by Cisco for wireless authentication.
  - Uses the RC4 encryption algorithm.
  - Vulnerable to dictionary attacks and has largely been replaced by more secure protocols.

- **PEAP**:
  - Provides secure authentication using TLS for encryption.
  - Supports server-side certificates for server authentication.
  - Offers stronger encryption algorithms (e.g., AES, 3DES) and is widely used in enterprise networks.

While protocols like SSH and HTTPS utilize SSL/TLS for secure authentication, they provide robust encryption to protect authentication data from interception and tampering. Both protocols support digital certificates and Public Key Infrastructure (PKI) for server authentication, helping prevent Man-in-the-Middle (MITM) attacks.

Overall, understanding these authentication protocols is crucial for implementing secure systems and protecting sensitive information in various networking environments.


# TCP/UDP Connections

**Transmission Control Protocol (TCP)** and **User Datagram Protocol (UDP)** are two fundamental protocols used for data transmission over the Internet. Each serves distinct purposes, with TCP primarily used for reliable data transfer (e.g., web pages, emails) and UDP employed for real-time data delivery (e.g., streaming video, online gaming).

## TCP vs. UDP

### TCP (Transmission Control Protocol)
- **Connection-Oriented**: Establishes a connection before data is sent, akin to a phone call where both parties are connected until the conversation ends.
- **Reliability**: Ensures that all data sent is received correctly. If an error occurs, the receiver will notify the sender to resend the missing data.
- **Flow Control**: Manages data flow to prevent overwhelming the receiver, contributing to a slower but more reliable transmission.

### UDP (User Datagram Protocol)
- **Connectionless**: Does not establish a connection before sending data; instead, it sends data directly to the target host without verifying its receipt.
- **Speed**: Prioritizes speed over reliability, making it suitable for applications where timely delivery is crucial, such as live streaming or gaming.
- **Loss Tolerance**: Accepts potential data loss, as there are no acknowledgments for received packets.

## IP Packet Structure

An **IP packet** is the basic unit of data sent over a network, containing a header and a payload.

### IP Header

The IP header includes several fields that provide critical information for routing and delivery:

| Field                    | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Version**              | Indicates the IP protocol version (IPv4 or IPv6).                         |
| **Internet Header Length**| Specifies the header length in 32-bit words.                              |
| **Class of Service**     | Indicates the priority of the packet.                                      |
| **Total Length**         | Specifies the total packet length in bytes.                                |
| **Identification (ID)**  | Identifies fragments of the packet if fragmented.                          |
| **Flags**                | Indicates fragmentation status.                                            |
| **Fragment Offset**      | Shows the position of the fragment within the original packet.             |
| **Time to Live (TTL)**   | Limits the packet's lifespan to prevent endless circulation.               |
| **Protocol**             | Specifies the upper-layer protocol (TCP, UDP, etc.).                       |
| **Checksum**             | Used for error-checking the header.                                       |
| **Source/Destination IP**| Indicates the sender and recipient IP addresses.                           |
| **Options**              | Contains optional routing information.                                     |
| **Padding**              | Pads the packet to ensure a full word length.                             |

## IP Record-Route Field

The **Record-Route** field records the path taken by the packet through the network. It lists the IP addresses of all routers the packet passed through when it reaches the destination. This can be observed using tools like `ping` with the `-R` option.

Example of ping output:
```bash
PING 10.129.143.158 (10.129.143.158) 56(124) bytes of data:

64 bytes from 10.129.143.158:
- ICMP Sequence: 1
- TTL: 63
- Time: 11.7 ms

Record Route:
- 10.10.14.38
- 10.129.0.1
- 10.129.143.158
- 10.10.14.1

```

This output indicates the route taken by the packet, helping in troubleshooting and analyzing network paths.

## Traceroute

**Traceroute** is a diagnostic tool that tracks the path packets take to reach a destination. It operates by sending TCP SYN packets with increasing TTL values to identify each hop along the route. When a packet's TTL reaches zero at a router, an ICMP Time-Exceeded message is returned, revealing the router's IP address.

### Process:
1. Send a TCP SYN packet with TTL = 1.
2. The router decrements TTL, dropping the packet and returning an ICMP Time-Exceeded.
3. Record the router's IP address.
4. Repeat with TTL incremented until the destination is reached.

## IP Payload

The **payload** of an IP packet contains the actual data being transmitted. For both TCP and UDP, this payload varies based on the protocol:

### TCP Payload
- **TCP Segments**: Composed of headers and payloads, with headers containing fields like source and destination ports, sequence numbers, acknowledgment numbers, and control flags.
  
### UDP Payload
- **UDP Datagrams**: Sent without establishing a connection, focusing on direct delivery without overhead for reliability checks.

## Blind Spoofing

**Blind spoofing** is an attack where an attacker sends falsified packets without seeing the responses. This involves manipulating the IP header to indicate false source and destination addresses, potentially allowing the attacker to disrupt connections or intercept information. For instance, sending a TCP packet with a false Initial Sequence Number (ISN) can trick the target into establishing a connection without valid authentication.

---

Understanding TCP/UDP connections and the structure of IP packets is crucial for network management, troubleshooting, and security practices. By leveraging these protocols effectively, network administrators can ensure reliable data transmission and robust network performance.



# Cryptography

Cryptography is essential for securely transmitting data over the Internet, such as payment information, emails, and personal data. It transforms data into an unreadable format for unauthorized users through various mathematical algorithms. Encryption methods can be classified into two main categories: **symmetric** and **asymmetric** encryption.

## Symmetric Encryption

- **Definition**: Uses the same key for both encryption and decryption.
- **Key Management**: The sender and receiver must securely share and manage the same key. If the key is compromised, the data's security is at risk.
- **Common Algorithms**: 
  - **Advanced Encryption Standard (AES)**: Currently considered the most secure symmetric algorithm.
  - **Data Encryption Standard (DES)**: An older algorithm with a 56-bit key length, now deemed insecure for many applications.
- **Use Cases**: Often used for encrypting large amounts of data, such as files on a hard drive or data sent over networks.

## Asymmetric Encryption

- **Definition**: Uses a pair of keys—public and private keys.
- **Functionality**: The public key encrypts data, while the private key decrypts it. This allows anyone to send encrypted data to the key owner without needing to share the private key.
- **Examples**: 
  - **RSA** (Rivest–Shamir–Adleman)
  - **PGP** (Pretty Good Privacy)
  - **ECC** (Elliptic Curve Cryptography)
- **Advantages**: Enhanced security based on complex mathematical problems, and eliminates the need for key exchange.

## Key Encryption Standards

- **Data Encryption Standard (DES)**: A symmetric-key block cipher that uses a 56-bit key for encryption. Due to vulnerabilities, it has largely been replaced by more secure algorithms.
- **Triple DES (3DES)**: An extension of DES that applies encryption three times, enhancing security but still limited by the 56-bit key length.
- **Advanced Encryption Standard (AES)**: A successor to DES, supporting key lengths of 128, 192, or 256 bits. AES is faster and more secure, widely used in applications like:
  - **WLAN IEEE 802.11i**
  - **IPsec**
  - **SSH**

## Cipher Modes

Cipher modes define how a block cipher processes data. Common modes include:

- **Electronic Code Book (ECB)**: Not recommended due to security vulnerabilities and inability to hide data patterns effectively.
- **Cipher Block Chaining (CBC)**: Default mode for AES; commonly used in disk encryption and secure communications (e.g., TLS).
- **Cipher Feedback (CFB)**: Suitable for real-time data encryption, such as network communications.
- **Output Feedback (OFB)**: Similar to CFB but generates a keystream that improves security for real-time communication.
- **Counter (CTR)**: Used for real-time data streams; efficient for high-speed encryption tasks.
- **Galois/Counter (GCM)**: Provides both confidentiality and integrity, used in secure communications like VPNs.

Each encryption mode has specific characteristics and applications, and the choice depends on the security requirements of the task at hand.
