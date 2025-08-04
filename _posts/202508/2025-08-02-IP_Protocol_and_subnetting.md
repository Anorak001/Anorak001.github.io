---    
title: Understanding IP Protocol and Subnetting for Beginners
description: >-
    A beginner-friendly guide to understanding IP protocols and subnetting, essential for anyone starting out in networking or cybersecurity.
author: anorak
date: 2025-08-02 04:30:00 +0530
categories: [GUIDE, COMPUTER NETWORKS]
tags: [Networking, CEH, Computer Networks]
pin: True
--- 

## What is IP?

- **IP (Internet Protocol)** is a foundational protocol at the Network layer (Layer 3) of the OSI model.
- It is responsible for addressing, routing, and delivering data packets across interconnected networks, forming the backbone of the Internet.
- As data traverses the network stack, each protocol layer adds its own **header**—a structured block of metadata that guides the packet’s journey.
- The **IP header** transforms data into a **packet** (the Protocol Data Unit, or PDU, for IP), containing all the information needed for delivery.

### IP Versions

- **IPv4**: The most widely deployed version, IPv4 uses 32 bits (4 octets) to represent addresses, supporting about 4.3 billion unique addresses.
    - IPv4 addresses are typically written in dotted decimal notation (e.g., `192.168.1.1`).
    - Due to the rapid growth of the Internet, IPv4 address exhaustion became a concern.
- **IPv6**: Developed to overcome IPv4 limitations, IPv6 uses 128 bits (16 bytes), enabling an almost limitless pool of addresses.
    - IPv6 addresses are written in hexadecimal and separated by colons (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`).
    - IPv6 also introduces features like simplified headers, improved security, and better support for mobile devices.

 

## IP Headers

![IP Headers](/assets/img/202508/img.jpg){: .center}   

- Every IP packet contains a **header** with fields that describe, manage, and route packets across networks.
- The structure and standards for IP headers are defined by the **Internet Engineering Task Force (IETF)** in documents called RFCs (Requests for Comments).
- **Key IPv4 header fields:**
    - **Version**: 4 bits, specifies the IP version (IPv4 or IPv6).
    - **Header Length**: 4 bits, indicates the length of the header in 32-bit words.
    - **Type of Service (ToS)**: 8 bits, used for Quality of Service (QoS) to prioritize certain types of traffic.
    - **Total Length**: 16 bits, specifies the entire packet size (header + data).
    - **Identification, Flags, Fragment Offset**: Used for packet fragmentation and reassembly when packets are too large for the underlying network.
    - **Time to Live (TTL)**: 8 bits, limits the number of hops a packet can take before being discarded, preventing infinite loops.
    - **Protocol**: 8 bits, identifies the next protocol (e.g., TCP, UDP, ICMP).
    - **Header Checksum**: 16 bits, used for error-checking the header.
    - **Source Address**: 32 bits, the sender’s IP address.
    - **Destination Address**: 32 bits, the receiver’s IP address.
- **IPv6 headers** are simpler, with fewer fields, but include extension headers for additional features.

> **Note:** The term **octet** (8 bits) is used instead of byte for clarity, as not all systems historically used 8-bit bytes.

 

## IP Addressing

- **IPv4 addresses** consist of 4 octets, each ranging from 0 to 255, separated by dots.
- **Special IPv4 address ranges:**
    - **Loopback**: 127.0.0.0–127.255.255.255 (commonly 127.0.0.1), used for testing and diagnostics on the local machine.
    - **Private networks** (RFC 1918):
        - 10.0.0.0–10.255.255.255
        - 172.16.0.0–172.31.255.255
        - 192.168.0.0–192.168.255.255
    - **Multicast**: 224.0.0.0–239.255.255.255, used for one-to-many communication.
    - **Reserved**: 240.0.0.0 and above, set aside for future or experimental use.
- **Public addresses** are globally routable, while private addresses are used within local networks and require NAT (Network Address Translation) for Internet access.

### IPv6 Addressing

- **IPv6 addresses** are 128 bits (16 bytes) long, written in hexadecimal and separated by colons.
    - Example: `fe80::62e3:5ec3:3e06:daa2`
- **Types of IPv6 addresses:**
    - **Unicast**: Identifies a single unique interface.
    - **Anycast**: Sent to the nearest member of a group of interfaces, based on routing distance.
    - **Multicast**: Sent to multiple interfaces simultaneously, replacing IPv4 broadcast.
- **No broadcast addresses** exist in IPv6; multicast is used instead.
- IPv6 supports **stateless address autoconfiguration (SLAAC)**, allowing devices to self-assign addresses.

 

## Subnetting

- **Subnetting** divides a larger IP address space into smaller, more manageable networks (subnets), improving organization, security, and efficiency.
- A **subnet mask** is a 32-bit number that indicates which part of an IP address refers to the network and which part refers to the host.
    - Example: `255.255.255.128` in binary is `11111111.11111111.11111111.10000000`
    - The number of 1s in the mask = network bits; the 0s = host bits.
- **CIDR Notation** (Classless Inter-Domain Routing) uses a slash and a prefix length to specify the network portion (e.g., `/25` means 25 bits for the network).
    - Example: `172.20.30.42/25` means the first 25 bits are the network, and the remaining bits are for hosts.
- **Calculating hosts per subnet:**
    - `/24` = 256 addresses (254 usable for hosts)
    - `/25` = 128 addresses (126 usable)
    - `/23` = 512 addresses (510 usable)
- **Network and broadcast addresses**: The first (network) and last (broadcast) addresses in each subnet cannot be assigned to hosts.
- **Subnetting benefits:**
    - Reduces broadcast domains, improving network performance.
    - Enhances security by isolating network segments.
    - Allows for efficient IP address allocation.

### Subnetting in IPv6

- IPv6 does not use subnet masks; only **CIDR notation** is used (e.g., `/50`).
- The prefix length defines the network portion, and the remaining bits are available for hosts, allowing for extremely large subnets.
- IPv6 subnets are typically much larger than IPv4, simplifying network design and management.

 

## Key Takeaways

- **IP** is the backbone of all networking, with IPv4 and IPv6 as the main versions in use today.
- **Headers** and **addressing** are crucial for routing, delivery, and management of network traffic.
- **Subnetting** allows for efficient organization, security, and control of network resources.
- Mastering these concepts is essential for pentesting, network troubleshooting, and effective network management.
- Understanding how IP works, how addresses are structured, and how subnetting operates will help you design, secure, and troubleshoot modern networks with confidence.
