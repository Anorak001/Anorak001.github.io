---
title: IDS Evasion Techniques
description: >-
 Blog on Most Common IDS Evasion Techniques
author: anorak
date: 2025-03-08 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Evasion Techniques]
pin: False
---

# Techniques to Evade Intrusion Detection Systems (IDS)

There are multiple different techniques to evade IDS. Below is a comprehensive breakdown of each method:

## 1. Insertion Attack

**Insertion:** IDS processes false data; host ignores it.

It is the process in which an attacker confuses the IDS by forcing it to read invalid packets (i.e. the end system will not accept the packet that is accepted by the IDS).

- If the packet is malformed or does not reach its actual destination, then the packet is invalid.
- If the IDS reads an invalid packet, it gets confused. Attackers exploit this condition and insert data into the IDS.

**Additional Details:**

- To understand how insertion becomes a problem for a network IDS, it is important to understand how the IDS detects attacks. It employs pattern-matching algorithms to look for specific patterns of data in a packet or stream of packets.
- For example, it might search for the “phf” string in an HTTP request to discover a PHF Common Gateway Interface (CGI) attack.
- An attacker who can insert packets into the IDS can prevent pattern matching from working. For instance, an attacker can send the string “phf” to a web server, attempting to exploit the CGI vulnerability, but force the IDS to read “phoneyf” (by “inserting” the string “oney”) instead.
- A straightforward insertion attack involves intentionally corrupting the IP checksum. Every packet transmitted on an IP network has a checksum that verifies the integrity of the packets.
- IP checksums are 16-bit numbers computed by examining the information in the packet.
- If the checksum on an IP packet does not match the actual packet, the addressed host will not accept it, while the IDS might consider it as part of the effective stream.
- For example, the attacker can send packets whose time-to-live (TTL) fields are crafted to reach the IDS but not the target computers.

 

## 2. Evasion Attack

**Evasion:** IDS ignores the attack data; host processes it.

- **Network Flow:** When a packet is sent across the network, both the IDS (Intrusion Detection System) and the host (the server or computer receiving the packet) inspect it.
- **Packet Handling Differences:** The key in evasion attacks is that the IDS and the host process packets differently.
- **How an Evasion Attack Works:** An attacker can exploit this difference by sending specially crafted packets that:
  - Look suspicious or invalid to the IDS, so it discards or ignores them (thinking they’re unimportant or irrelevant).
  - But these same packets are reassembled and accepted by the host (because the host interprets them differently). The host, however, may have different rules or be more flexible in how it handles these packets.

 

## 3. Denial-of-Service Attack (DoS)

An IDS requires memory for various tasks such as generating a match for the patterns, saving the TCP connections, maintaining reassembly queues, and producing buffers for the data.

- **Memory Allocation:** In the initial phase, the system requires memory to read the packets. The system will allocate the memory for network processing operations.
- **Exploitation:** An attacker can force the IDS to assign all of its memory for meaningless information.
- **Disk Storage:** In certain circumstances, the IDS stores activity logs on the disk. The stored events occupy most of the disk space. Most computers have limited disk space.
  - The attackers can occupy a significant part of the disk space on the IDS by creating and storing a large number of useless events. This renders the IDS useless in terms of storing real events.
- **Overloading the Network:** The IDS, unlike an end system, must read everyone’s packets, not just those explicitly sent to it. An attacker can overload the network with meaningless information and prevent the IDS from keeping up with what is happening on the network.
- **False Positive Generation:** Mode of false positive generation does not attack the target; instead, it does something relatively ordinary.
  - In this mode, the IDS generates an alarm when no condition is present to warrant one.
  - Another attack similar to the DoS method is to create a significant amount of alert data that the IDS will log.
  - Attackers construct malicious packets known to trigger alerts within the IDS, forcing it to generate a large number of false reports.
  - Such an attack creates a large amount of log “noise” in an attempt to blend real attacks with fake ones.
  - Attackers can generate false positives specific to that IDS and then use these false positive alerts to hide real attack traffic.
  - As a result, attackers can bypass the IDS unnoticed, as it is difficult to differentiate the attack traffic from the large volume of false positives.

 

## 4. Obfuscating

Obfuscation means to make code more difficult to understand or read, generally for privacy or security purposes. A tool called an obfuscator converts a straightforward program into one that works in the same way but which is much more difficult to understand.

- **IDS Evasion:** Obfuscating is an IDS evasion technique used by attackers to encode the attack packet payload in such a way that the destination host can only decode the packet but not the IDS.
- **Unicode Encoding:** Using Unicode characters, an attacker can encode attack packets that the IDS would not recognize but which an (Internet Information Services) IIS web server can decode.
- **Polymorphic Code:** 
  - Polymorphic code is a type of malicious code (typically associated with viruses or malware) that mutates or changes its appearance each time it is executed, but its underlying functionality remains the same.
  - The goal of polymorphic code is to avoid detection by signature-based security systems, such as Intrusion Detection Systems (IDS) or antivirus programs, by constantly changing its structure.
- **ASCII Shellcodes:** 
  - ASCII shellcodes contain only characters from the ASCII standard.
  - Such shellcodes allow attackers to bypass commonly enforced character restrictions within the string input code.
  - They also help attackers bypass IDS pattern matching signatures because they hide strings similarly to polymorphic shellcodes.

**Another Example:**

- **What is Unicode?:** It’s a system that allows computers to represent text from many languages consistently. It uses different ways to encode characters, like UTF-8 and UTF-16.
- **How does the attack work?:** Attackers can use Unicode to hide malicious code from Intrusion Detection Systems (IDS). For example, the same character (like “/”) can be written in multiple ways using different Unicode codes, like “%u2215” or “%5C.” This makes it hard for the IDS to match the character with attack signatures.
- **Why is this effective?:** Since Unicode allows many representations for the same character, the IDS may not recognize the attack when characters are encoded in a tricky way. Attackers can convert harmful code into these multiple forms to bypass detection.

 

## 5. Session Splicing

Session splicing is an IDS evasion technique that exploits how some IDS do not reconstruct sessions before pattern-matching the data.

- **Splitting the Attack:** Instead of sending a large malicious packet that the IDS can easily recognize, the attacker breaks the attack data into small pieces, each sent in separate packets.
- **Evading Detection:** The IDS, which looks for known attack patterns (signatures), cannot detect the attack because each small packet by itself does not trigger any alarms. It doesn’t match the full malicious pattern since the data is fragmented.
- **Exploiting IDS Weaknesses:** Some IDS systems do not properly reassemble these small packets back into their original form before checking for attacks. As a result, the full attack goes undetected.
- **Delays to Avoid Detection:** If the attacker knows the IDS is reassembling packets slowly, they can add delays between the small packets, further confusing the IDS and preventing it from spotting the attack.
- **Timeout Exploitation:** If the application under attack keeps a session active for a longer time than that spent by the IDS on reassembling it, the IDS will stop. As a result, any session after the IDS stops reassembling the sessions will be susceptible to malicious data theft by attackers.
- **Fragmentation Attack:** A similar fragmentation attack works when the IDS timeout exceeds that of the victim.
  - IP packets must follow the standard Maximum Transmission Unit (MTU) size while traveling across the network. If the packet size is exceeded, it will be split into multiple fragments (“fragmentation”).
  - The IP header contains a fragment ID, fragment offset, fragment length, fragment flags, and other information besides the original data.
  - In a network, the flow of packets is irregular; hence, systems need to keep fragments around, wait for future fragments, and then reassemble them in order.
- **Tools:** Attackers can use tools such as Nessus for session-splicing attacks.

- **TCP Checksum and RST Exploitation:**
  - TCP uses 16-bit checksums to verify the integrity of data and headers, ensuring reliable communication.
  - **Checksum Function:** Each TCP segment includes a checksum that the receiving host checks. If the received checksum doesn’t match the expected value, the packet is discarded.
  - **Ending Communication:** TCP uses RST (reset) packets to terminate a communication session.
  - **Exploitation by Attackers:** Attackers can send RST packets with an invalid checksum. This tricks the Intrusion Detection System (IDS) into thinking the communication session has ended, causing it to stop processing the data stream.
  - **Host Response:** The target host, however, checks the checksum and will drop the invalid RST packet, meaning the actual communication can continue while the IDS is misled.

 

## 6. Overlapping Fragments

- **Overlapping Data:** Attackers send several fragments of a message where some parts overlap. For example, the first fragment has 100 bytes starting at sequence number 1, and the second fragment overlaps with 96 bytes of the first one.
- **Confusing the IDS:** The IDS has trouble deciding how to reassemble these overlapping fragments, which may allow the attacker to bypass detection.

 

## 7. Time to Live Attack

- **Time to Live (TTL):** Each data packet has a TTL value that tells how many “hops” (routers) it can pass through before it gets dropped. When TTL reaches zero, the packet is discarded, and an ICMP alert notification is sent to the sender.
- **OS Identification:** Different operating systems (OS) have different starting TTL values, which can give away the OS type to attackers.
- **SmartDefense:** Some systems like SmartDefense can change the TTL field of all packets (or all outgoing packets) to a given number.
- **Attack Setup:**
  - The attacker has prior knowledge of the topology of the victim’s network. This information can be obtained using tools such as traceroute, which gives information on the number of routers between the attacker and the victim.
- **How the Attack Works:**
  - **Step 1:** The attacker sends the first fragment (frag-1) with a high TTL, so it reaches both the IDS and the victim.
  - **Step 2:** The attacker sends frag-2' with a TTL of 1, so it only reaches the IDS (it gets dropped by the router before reaching the victim). Frag-2' has false data.
  - **Step 3:** The attacker sends frag-3 with a higher TTL, so it reaches both the IDS and the victim. The IDS thinks it has received all parts of the message (frag-1, frag-2', and frag-3) and tries to reassemble them, but the reassembled data is fake.
  - **Final Step:** The attacker sends the real frag-2 to the victim, who waits for this valid piece. Once the victim receives it, they reassemble the correct message, which contains the malicious attack code.

 

## 8. Urgency Flag Attack

- **Urgent Data:** The urgency flag in TCP signals that certain data within a packet is urgent and should be processed immediately.
- **How It Works:**
  - When a user sets the urgency flag, TCP uses an urgency pointer to indicate where the urgent data starts within the packet. Any data before this pointer is ignored for urgent processing.
  - If the URG flag is set, TCP marks the position of the last byte of urgent data with a 16-bit offset value.
- **IDS Behavior:**
  - Some Intrusion Detection Systems (IDS) don’t recognize the urgency feature and process all packet data, while the target system only focuses on the urgent data.
- **Exploitation:**
  - Attackers can take advantage of this by adding irrelevant (or “garbage”) data before the urgent data.
  - The IDS reads all this extra data, but the target system only processes the urgent part.
  - As a result, the IDS sees more data than the target system, creating a situation where attackers can send malicious traffic that goes undetected by the IDS.

 

## 9. Application-Layer Attacks

- **Compressed Media Files:** Media files such as images, audios, and videos can be compressed so that they can be rapidly transferred as smaller chunks.
- **Attack Method:** Attackers find flaws in this compressed data and perform attacks; even the IDS signatures cannot identify the attack code within data thus compressed.

 

## 10. Desynchronization

In the Post-Connection SYN attack, attackers send a SYN packet during an ongoing connection, using sequence numbers that don’t match the established session.

- **Post-Connection SYN Attack:**
  - The target host ignores this SYN because the connection is already active.
  - However, the IDS may update its tracking to match the new, incorrect sequence numbers.
  - As a result, the IDS will overlook legitimate data in the original stream, waiting instead for the sequence numbers from the attacker’s SYN.
  - Once the IDS is resynchronized, the attacker sends a RST (reset) packet with the new sequence number to terminate the IDS’s connection tracking, effectively confusing it further.
- **Pre-Connection SYN Attack:**
  - The attacker sends a fake SYN packet with an incorrect TCP checksum before establishing a real connection.
  - When the IDS receives a new SYN after a connection is opened, it adjusts its tracking based on the new packet.
  - The attacker sends these fake SYN packets with random sequence numbers to disrupt the IDS, making it unable to monitor real traffic properly.
  - If the IDS doesn’t check the TCP checksum, it might miss the attack. If it does check, the attacker sends a fake sequence number to the IDS before the real connection starts.
- **Summary:** Pre-Connection attacks disrupt IDS monitoring before connections are established, while Post-Connection attacks aim to manipulate the IDS after a connection is already open.

 

## 11. Encryption

- **Network-Based IDS Limitations:** Network-based intrusion detection analyzes traffic in the network from the source to the destination.
- **Encrypted Sessions:** If an attacker succeeds in establishing an encrypted session with his/her target host using protocols such as secure shell (SSH), secure socket layer (SSL), or virtual private network (VPN) tunnel, the IDS will not analyze the packets going through these encrypted communications.
- **Result:** An attacker sends malicious traffic using such secure channels, thereby evading IDS security.

 

This detailed breakdown covers multiple techniques attackers might use to evade IDS monitoring. Each method exploits specific weaknesses or assumptions within the IDS’s operation, making it challenging to detect and prevent advanced attacks.

