---
title: How Malwares evade Traditional AV Software
description: >-
 This is a detailed article on how LOTL attacks are executed and ways of evading them.
author: anorak
date: 2024-08-04 16:55:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity,malware]
pin: false
---

## Introduction:
Trust me! This is the best AV Out there!
![Defender](/assets/img/202408/defender.jpeg){: width="800" height="700" .w-50 .right}


Jokes Apart, You might run your expensive antivirus scans regularly and find some malicious files too. However, there is a technique called "living off the land attacks," which specifically uses native system files to hide the malware so that it blends in and antivirus software fails to detect it.
This article provides all sorts of information on LOTL attacks.

Living off the land (LOTL) is a fileless malware or LOLbins cyberattack technique where the cybercriminal uses native, legitimate tools within the victim’s system to sustain and advance an attack.
 
## How does an LOTL Attack Work?

- Unlike traditional malware attacks, which leverage signature files to carry out the attack plan, LOTL attacks are fileless — meaning they do not require an attacker to install any code or scripts within the target system.
- Instead, the attacker uses tools that are already present in the environment, such as PowerShell, Windows Management Instrumentation (WMI) or the password-saving tool, Mimikatz, to carry out the attack.

- Using native tools makes LOTL attacks far more difficult to detect, especially if the Individual/organization is leveraging traditional security Software that searches for Known types of Malware from their Database.
- Because of this gap in the security toolset, the hacker is often able to stay undetected in the victim’s environment for weeks, months or even years.

- You might have this question of how does the hacker gain access to the native environment of the target eventhough he doesn't install anything else in the host system.
- The Hacker uses various tools and methods to gain access to the native environment of the host by using the following techniques:
    - [Hijacked Native Tools](#hijacked-native-tools)
    - [Fileless Ransomware](#fileless-ransomware)
    -  [Stolen Credentials](#stolen-credentials)
    -  [Exploit Kits](#exploit-kits)
    -  [Registry resident malware](#registry-resident-malware)

### Hijacked Native Tools
  In LOTL attacks, adversaries commonly hijack legitimate tools to escalate privileges, access different systems and networks, steal or encrypt data, install malware, set backdoor access points or otherwise advance the attack path. Examples of native or dual use tools include:

  - File transfer protocol (FTP) clients or system functions, such as PsExec
  - Forensic tools such as the password extracting tool Mimikatz
  - PowerShell, a script-launching framework that offers broad functionality for Windows device administration
  - WMI, an interface for access to various Windows components

### Fileless Ransomware

- Attackers are using fileless techniques to embed malicious code in documents through the use of native scripting languages such as macros or to write the malicious code directly into memory through the use of an exploit.
- The ransomware then hijacks native tools like PowerShell to encrypt hostage files without ever having written a single line to disk.

### Stolen Credentials

- Attackers may commence a fileless attack through the use of stolen credentials so they can access their target under the guise of a legitimate user. 
- Once inside, the attacker can use native tools such as WMI or PowerShell to conduct their attack. They can establish persistence by hiding code in the registry or the kernel, or by creating user accounts that grant them access to any system they choose.

### Exploit Kits

- Exploits are pieces of code, sequences of commands, or collections of data; exploit kits are collections of exploits. Adversaries use these tools to take advantage of vulnerabilities that are known to exist in an operating system or an installed application.
- An exploit begins in the same way, regardless of whether the attack is fileless or uses traditional malware. Typically, a victim is lured through a phishing email or social engineering. The exploit kit usually includes exploits for a number of vulnerabilities and a management console that the attacker can use to control the system.
- In some cases, the exploit kit will include the ability to scan the targeted system for vulnerabilities and then craft and launch a customized exploit on the fly.

### Registry Resident Malware:

- Registry resident malware is malware that installs itself in the Windows registry in order to remain persistent while evading detection.
- Commonly, Windows systems are infected through the use of a dropper program that downloads a malicious file. This malicious file remains active on the targeted system, which makes it vulnerable to detection by antivirus software. Fileless malware may also use a dropper program, but it doesn’t download a malicious file. Instead, the dropper program itself writes malicious code straight into the Windows registry.
- The malicious code can be programmed to launch every time the OS is launched, and there is no malicious file that could be discovered — the malicious code is hidden in native files not subject to AV detection.
- The oldest variant of this type of attack is Poweliks, but many have emerged since then, including Kovter and GootKit. Malware that modifies registry keys is very likely to remain in place undetected for extended periods of time. 


## Why are these LOTL attacks so appealing to Cybercriminals?


  - Many common LOTL attack vehicles, such as WMI and PowerShell, are in the victim network’s “allow” list, which makes for a perfect cover for adversaries as they carry out malicious activity — activity that is often ignored by the victim’s security operations center (SOC) and other security measures.
  - LOTL attacks do not use files or signatures, which means that attacks cannot be compared or connected, making it more difficult to prevent in the future and allowing the criminal to reuse tactics at will.
  - Use of legitimate tools and lack of signature makes it difficult to attribute LOTL attacks, thus fueling the attack cycle.
  - Lengthy, undisturbed dwell times allow the adversary to set up and carry out complex, sophisticated attacks. By the time the victim is aware of the issue, there is often little time to effectively respond.



## Prevention:
 - To avoid these attacks, we must concentrate on IOAs ( Indicators Of Attack) rather than IOCs ( Indicators of Compromise) alone.
 - Indicators of attacks are a more proactive detection capability that look for signs that an attack may be in progress. IOAs include signs such as code execution, lateral movements and actions that seem to be intended to cloak the intruder’s true intent.
 - Because fileless attacks exploit legitimate scripting languages such as PowerShell and are never written to disk themselves, they go undetected by signature-based methods, allowlisting and sandboxing. Even machine learning methods fail to analyze fileless malware. But IOAs look for sequences of events that even fileless malware must execute in order to achieve its mission.
 - IOAs examine intent, context and sequences, they can even detect and block malicious activities that are performed using a legitimate account, which is often the case when an attacker uses stolen credentials or hijacks legitimate programs. 



























































































































