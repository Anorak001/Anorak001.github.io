---
title: Perform Network Recon like a Pro!
description: >-
 A blog on most popular recon methods
author: anorak
date: 2025-01-04 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity, Networking, Reconnaissance, Ethical-Hacking, Penetration-Testing, Network-Security, Tools]
pin: false
---


## Reconnaissance  

Reconnaissance is an important first stage in any ethical hacking attempt. Before it’s possible to exploit a vulnerability in the target system, it’s necessary to find it. By performing reconnaissance on the target, an ethical hacker can learn about the details of the target network and identify potential attack vectors.

Reconnaissance efforts can be broken up into two types:  
- Passive reconnaissance prioritizes subtlety (ensuring that the hacker is not detected).  
- Active reconnaissance is used for cases where collecting information is more important than remaining undetected.  

---

## Top Passive Recon Tools  

In passive reconnaissance, the hacker never interacts directly with the target’s network. The tools used for passive reconnaissance take advantage of unintentional data leaks from an organization to provide the hacker with insight into the internals of the organization’s network.  

- **Wireshark**  
  Wireshark is best known as a network traffic analysis tool, but it can also be invaluable for passive network reconnaissance.  
  - If an attacker can gain access to an organization’s Wi-Fi network or otherwise eavesdrop on the network traffic of an employee (e.g., by eavesdropping on traffic in a coffee shop), analyzing it in Wireshark can provide a great deal of useful intelligence about the target network.  
  - By passively eavesdropping on traffic, a hacker may be able to map IP addresses of computers within the organization’s network and determine their purposes based on the traffic flowing to and from them.  
  - Captured traffic may also include version information of servers, allowing a hacker to identify potentially vulnerable software that can be exploited.  

- **Google**  
  Google can provide a vast amount of information on a variety of different topics.  
  - The information that an organization posts online can provide a massive amount of information about their network.  
  - The organization’s website, especially its career page, can provide details of the types of systems used in the network.  
  - Using specialized Google queries (Google Dorking), it’s possible to search for files that were not intentionally exposed to the internet but are still publicly available.  

- **FindSubDomains.com**  
  FindSubDomains.com is one example of a variety of different websites designed to help identify websites that belong to an organization.  
  - Many of these sites may be deliberately intended for public consumption, while others may be protected by login pages.  
  - Some may be unintentionally exposed to the internet, providing valuable intelligence about the systems the company uses.  

- **VirusTotal**  
  VirusTotal is a website designed to help with the analysis of potentially malicious files.  
  - The problem is that the service makes the same information available to any free subscriber (and provides more data to paid users).  
  - A hacker searching through the data provided on VirusTotal by keywords associated with a company can potentially find a great deal of valuable intelligence.  

- **Shodan**  
  Shodan is a search engine for internet-connected devices.  
  - Using Shodan, a hacker may be able to find devices within the IP address range belonging to a company, indicating that they have the device deployed on their network.  
  - Since many IoT devices are vulnerable by default, identifying one or more on the network may give a hacker a good starting point for a future attack.  

---

## Top Active Recon Tools  

Tools for active reconnaissance are designed to interact directly with machines on the target network in order to collect data that may not be available by other means. Active reconnaissance can provide a hacker with much more detailed information about the target but also runs the risk of detection.  

- **Nmap**  
  Nmap is a network scanner designed to determine details about a system and the programs running on it.  
  - This is accomplished through a suite of different scan types that take advantage of the details of how a system or service operates.  
  - By launching scans against a system or a range of IP addresses under a target’s control, a hacker can learn significant information about the target network.  

- **Nessus**  
  Nessus is a commercial vulnerability scanner.  
  - Its purpose is to identify vulnerable applications running on a system and provide a variety of details about potentially exploitable vulnerabilities.  

- **OpenVAS**  
  OpenVAS is a vulnerability scanner developed in response to the commercialization of Nessus.  
  - It provides a lot of the same functionality as Nessus but may lack some features developed since Nessus became a paid product.  

- **Nikto**  
  Nikto is a web server vulnerability scanner that can detect a variety of vulnerabilities.  
  - It is not a stealthy scanner, and its scans are easily detectable by intrusion detection or prevention systems.  

- **Metasploit**  
  Metasploit is primarily designed as an exploitation toolkit but can also be effectively used for reconnaissance.  
  - Using the autopwn option allows a hacker to try to exploit a target using any means necessary.  
  - More targeted analysis can allow a hacker to perform reconnaissance using Metasploit with more subtlety.  

---

## Conclusion: Performing Network Reconnaissance  

Network reconnaissance is a crucial part of any hacking operation. Any information that a hacker can learn about the target environment can help in identifying potential attack vectors and targeting exploits to potential vulnerabilities. By using a combination of passive and active reconnaissance tools and techniques, a hacker can maximize the information collected while minimizing their probability of detection.
