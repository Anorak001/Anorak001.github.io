---
title: LLMNR Poisoning
description: >-
 Detailed blog about LLMNR Poisoning and how to prevent it in AD
author: anorak
date: 2025-02-08 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity,techniques]
pin: False
---


## Overview

Active Directory (AD) stands as a foundational piece for many organizational networks, streamlining administrative tasks and enhancing productivity. However, out of the box, AD comes bundled with various features and default settings that can be exploited by attackers. Notably, protocols like LLMNR can pose significant security risks, especially for organizations that have never undergone a penetration test. This blog delves deep into the intricacies of LLMNR and the vulnerabilities it introduces, offering insights into its potential impacts and mitigation strategies.

## What is LLMNR?

LLMNR is a protocol that allows both IPv4 and IPv6 hosts to perform name resolution for hosts on the same local network without requiring a DNS server or DNS configuration.

When a host’s DNS query fails (i.e., the DNS server doesn’t know the name), the host broadcasts an LLMNR request on the local network to see if any other host can answer.

LLMNR is the successor to NetBIOS. NetBIOS (Network Basic Input/Output System) is an older protocol that was heavily used in early versions of Windows networking. NBT-NS is a component of NetBIOS over TCP/IP (NBT) and is responsible for name registration and resolution. Like LLMNR, NBT-NS is a fallback protocol when DNS resolution fails. It allows local name resolution within a LAN.

## How is Active Directory vulnerable to LLMNR?
![img](/assets/img/202502/llmnr-overview.png){: width="1000" height="1000"  .center}

LLMNR has no authentication mechanism. Anyone can respond to an LLMNR request, which opens the door to potential attacks. When a computer tries to resolve a domain name and fails via the standard methods (like DNS), it sends an LLMNR query across the local network. An attacker can listen for these queries and respond to them, leading to potential unauthorized access in the Active Directory environment.


## Exploiting LLMNR (AKA LLMNR Poisoning) in Active Directory

LLMNR poisoning is an attack where a malicious actor listens for LLMNR requests and responds with their own IP address (or another IP of their choosing) to redirect the traffic. This can lead to credential theft and relay attacks in Active Directory. Here is a sample walkthrough.
### Step 1: The Attacker runs Responder
    
   ```
    sudo responder -I eth0 -dwP
  ```
   ![img](/assets/img/202502/smb-3.png){: width="1000" height="1000"  .center}


### Step 2: An Event Occurs in the Network and Triggers LLMNR

 ![img](/assets/img/202502/llmnr-2-980x304.png){: width="800" height="800"  .center}
llmnr event in responder reveals ip address, domain, username, and password hash

When a LLMNR event occurs in the network and is maliciously responded to, the attacker will obtain sensitive information, including:

   The IP address of the victim (in this example: 10.0.3.7)
   The domain and username of the victim (in this example: MARVEL\fcastle)
   The victim’s password hash

With the victim’s hash in hand, we can attempt to take the hash offline and crack it.

### Step 3: Cracking the Victim’s Hash

We can now use a password cracking tool, such as Hashcat, to attempt to crack the victim’s hash.

``` hashcat –m 5600 <hashfile.txt> <wordlist.txt> ```

 ![img](/assets/img/202502/llmnr-3-1280x330.png){: width="800" height="800"  .center}

 

We have successfully cracked the victim’s password hash, which was found to be “Password1“.

## How can LLMNR Poisoning be Mitigated in Active Directory?

Main Defense – Disable LLMNR and NBT-NS

To disable LLMNR, select “Turn OFF Multicast Name Resolution” under Computer Configuration > Administrative Templates > Network > DNS Client in the Group Policy Editor of Active Directory.


 ![img](/assets/img/202502/llmnr-4-1280x554.png){: width="800" height="800"  .center}


 To disable NBT-NS, navigate to Network Connections > Network Adapter Properties > TCP/IPv4 Properties > Advanced tab > WINS tab and select “Disable NetBIOS over TCP/IP” in Active Directory. This only works locally.


 
 ![img](/assets/img/202502/llmnr-5-1280x674.png){: width="800" height="800"  .center}

 

To disable NBT-NS via GPO in Active Directory, we can simply write a PowerShell script (see below) and save it in Startup Scripts.

```
set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces\tcpip* -Name NetbiosOptions -Value 2
```

 ![img](/assets/img/202502/llmnr-6-1280x744.png){: width="800" height="800"  .center}

 Now add the script to Startup Scripts in Computer Configuration > Policies > Windows Settings > Scripts > Startup
 ![img](/assets/img/202502/llmnr-7-1280x784.png){: width="800" height="800"  .center}

 
## Confirming Our Mitigation

We can confirm that we have mitigated LLMNR by running the following command in PowerShell and receiving a ‘0’ in return:
```
$(Get-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows NT\DNSClient" -name EnableMulticast).EnableMulticast
```
 ![img](/assets/img/202502/llmnr-8-1280x164.png){: width="800" height="800"  .center}

 

We can confirm that we have mitigated NBT-NS by running the following command in cmd.exe and receiving a ‘2’ in return:
```
wmic nicconfig get caption,index,TcpipNetbiosOptions
 ```
 ![img](/assets/img/202502/llmnr-9-1280x468.png){: width="800" height="800"  .center}


## Alternate Defenses

If a company must use or cannot disable LLMNR/NBT-NS in Active Directory, the best course of action is to:

  - Require Network Access Control.
  - Require strong user passwords (e.g., >14 characters in length and limit common word usage). The more complex and longer the password, the harder it is for an attacker to crack the hash.


