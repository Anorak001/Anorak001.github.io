---  
title: Amadey Malware on Github
description: >-
    Learn how Hackers Used GitHub Repositories to Host Amadey Malware and Data Stealers, Bypassing Filters
author: anorak
date: 2025-07-20 04:30:00 +0530
categories: [NEWS, ISSUE]
tags: [Security, Cybersecurity, Exploitation, Open Source, Malware,   Threat Intelligence, GitHub]
pin: False
---

## Amadey Malware Campaign Leveraging GitHub

Hackers are increasingly abusing trusted platforms like GitHub to distribute malware and evade security controls. In April 2025, researchers observed a sophisticated campaign using public GitHub repositories to host and deliver the Amadey malware, along with various data stealers and attack tools. This post breaks down the tactics, tools, and broader trends behind these attacks.

 

### How Attackers Used GitHub to Distribute Malware

Threat actors created fake GitHub accounts to host malicious payloads, tools, and Amadey plug-ins. This approach helps them:

- Bypass web filtering by leveraging a trusted platform.
- Easily update or swap out payloads.
- Distribute malware at scale.

**Key points:**
- Attackers used a malware loader called Emmenhtal (aka PEAKLIGHT) to deliver Amadey.
- Amadey then downloaded additional payloads from the attackers' GitHub repositories.
- The campaign shares similarities with earlier phishing attacks that used invoice and billing lures to deliver other malware like SmokeLoader.

 

### Amadey and Emmenhtal: Malware Capabilities

Both Emmenhtal and Amadey act as downloaders for secondary payloads, but they have distinct features:

- **Emmenhtal:** Primarily a loader for other malware.
- **Amadey:** Can collect system information and be extended with DLL plugins for tasks like credential theft or screenshot capture.
- Amadey has also been used to deliver ransomware such as LockBit 3.0.

 

### GitHub Accounts and Payloads Identified

Cisco Talos researchers identified three GitHub accounts (Legendary99999, DFfe9ewf, and Milidmdds) used to host:

- Amadey plugins and secondary payloads.
- Malicious scripts for Lumma Stealer, RedLine Stealer, and Rhadamanthys Stealer.
- JavaScript files similar to those used in previous campaigns, with updated payloads.
- A Python script with embedded PowerShell to download Amadey from a hard-coded IP.

These accounts have since been removed by GitHub.

 

### Evolution of Attack Techniques

Attackers are constantly refining their methods. For example:

- Some scripts in the repositories were updated versions of those used in earlier campaigns.
- The use of Python and PowerShell demonstrates a shift toward multi-stage, cross-platform delivery.

 

### Broader Trends: Phishing and Loader Campaigns

The Amadey campaign is part of a larger trend of using phishing and loaders to deliver malware. Recent campaigns include:

- **SquidLoader:** Used in attacks against financial institutions in Hong Kong, Singapore, and Australia.
    - Employs anti-analysis, anti-sandbox, and anti-debugging techniques.
    - Deploys Cobalt Strike beacons for remote access.
    - Communicates with remote servers and injects next-stage payloads.
    ![Squid loader attack chain](/assets/img/202507/ss.webp){: .center}   

 

### Notable Social Engineering and Malware Delivery Tactics

Attackers are using a wide range of social engineering techniques to distribute malware. Some examples:

- **Invoice-themed phishing:** Delivers malicious droppers that install remote access tools like ConnectWise ScreenConnect.
- **Tax-related lures:** Trick users into installing malware disguised as PDF documents.
- **SSA-themed attacks:** Harvest credentials or install trojanized remote access tools, sometimes targeting SMS-based two-factor authentication.
- **Phishing kits:** Use services like Logokit and AWS to create convincing fake login pages, sometimes with CAPTCHA for added legitimacy.
- **Custom phishing kits:** Python Flask-based kits for easy credential theft.
- **QR code phishing ("Scanception"):** PDF attachments with QR codes that lead to fake Microsoft login pages.
- **ClickFix tactic:** Delivers stealers and remote access trojans.
- **Cloaking-as-a-service:** Hides malicious sites from security scanners.
- **HTML/JavaScript email attacks:** Craft realistic emails that bypass detection.
- **SVG image phishing:** Embeds obfuscated JavaScript in SVG files to redirect victims.

 

### The Rise of Advanced Phishing Techniques

According to Cofense, QR code phishing accounted for 57% of campaigns using advanced tactics in 2024. Other common methods include:

- Password-protected archive attachments to bypass secure email gateways.
- Increasing use of obfuscation and trusted platforms to evade detection.

 

By understanding these evolving tactics, organizations can better defend against the latest waves of malware and phishing attacks leveraging trusted platforms like GitHub.