## Infrastructure Design
One of the core responsibilities of a DevOps engineer is to design and implement infrastructure that supports software development and deployment. This includes:

- Setting up networks
- Configuring routers and firewalls
- Ensuring servers and other devices are properly connected

Understanding networking protocols and infrastructure design principles is essential for designing an efficient and scalable infrastructure.

## Application Deployment
DevOps engineers are responsible for deploying applications to production environments. This involves:

- Setting up and configuring servers
- Configuring load balancers and other network components
- Ensuring the application runs smoothly and reliably

Understanding networking principles is essential for configuring network components and resolving network-related issues during application deployment.

## Automation
DevOps is all about automating processes to improve efficiency and reduce errors. Networking automation tools, such as **Ansible** and **Puppet**, are used to automate the configuration of network devices and ensure that they are properly configured and maintained. A good understanding of networking protocols and automation tools is essential for automating network-related tasks.

## Monitoring
DevOps engineers are responsible for monitoring and maintaining the infrastructure and applications they manage. This includes:

- Monitoring network traffic
- Identifying bottlenecks and performance issues
- Troubleshooting network-related problems

Understanding networking protocols and tools is essential for identifying and resolving network issues in a timely manner.

---

# The OSI Model

The OSI model is a conceptual framework for understanding how data is transmitted across a network. It consists of seven layers:

1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

### Functions of Layers

**Physical Layer**  
- Physical characteristics of interfaces and media  
- Representation of bits  
- Data rate  
- Synchronization of bits  
- Line configuration (point-to-point or multi-point)  
- Transmission Mode  
- Physical Topology  

**Data Link Layer**  
- Framing  
- Physical addressing  
- Error control  
- Flow control  
- Access control  

**Network Layer**  
- Routing  
- Congestion control  
- Billing  

**Transport Layer**  
- Service Point addressing  
- Segmentation and reassembly  
- Flow control  
- Error control  

**Session Layer**  
- Dialog control  
- Synchronization  

**Presentation Layer**  
- Data encoding  
- Encryption  
- Compression  

**Application Layer**  
- File Transfer  
- Mail services  
- Directory services


  
---

# TCP/IP Reference Model

- **OSI vs TCP/IP**  
TCP/IP stands for **Transmission Control Protocol/Internet Protocol** and is a suite of communication protocols used to interconnect network devices on the Internet. It is also used in private networks (intranets or extranets).
<img width="343" height="417" alt="image" src="https://github.com/user-attachments/assets/70def1c0-92fd-4dea-8d94-a149c79372a4" />

---

# The IP Protocol

At the network layer, the Internet can be viewed as a collection of subnetworks or Autonomous Systems that are connected together. The network layer protocol used for the Internet is the **Internet Protocol (IP)**.  

Its job is to provide a best-effort way to transport datagrams from source to destination, regardless of whether the machines are on the same network or not.

### Communication Process
- Each datagram is transmitted from the Transport layer through the Internet
- It may be fragmented into smaller units
- Fragments are reassembled by the network layer at the destination

> **Note:** Datagrams are packets in the IP layer.

### Datagram Structure
A datagram is a variable-length packet (up to 65,536 bytes) consisting of **Header** and **Data**.

#### Header Fields
- **Version**: Current version is 4 (IPv4)  
- **Header Length (HLEN)**: Length of the header in multiples of 4 bytes (max 60 bytes) 
- **Service Type**: Priority and type of service (throughput, reliability, delay)  
- **Total Length**: Total length of datagram (max 65,535 bytes)  
- **Identification**: Used for fragmentation  
- **Flags**: Deal with fragmentation  
- **Fragmentation Offset**: Shows offset of data in original datagram  
- **Protocol**: Upper-layer protocol (TCP, UDP, ICMP, etc.)  
- **Time to Live (TTL)**: Number of hops before discard  
- **Header Checksum**: Checks integrity of header  
- **Source Address**: 32-bit Internet address of source  
- **Destination Address**: 32-bit Internet address of destination  
- **Options**: Additional functionality (routing, timing, management, alignment)  

---

# Addressing

In addition to the physical address, the Internet requires an additional addressing convention:  

- **Internet Address**: 4 bytes defining three fields:
  - Class Type
  - NetID
  - HostID  

These parts vary in length depending on the class of the address.

<img width="479" height="249" alt="image" src="https://github.com/user-attachments/assets/4b9598f2-66c0-44ea-bf50-b6159b5fdb76" />

