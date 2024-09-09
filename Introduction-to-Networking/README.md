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
