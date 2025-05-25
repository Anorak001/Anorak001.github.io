---
title: T1497 Technique
description: >-
 Detailed article on the famous sandox/VM evasion technique
author: anorak
date: 2024-12-08 00:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity,malware]
pin: false
---

Virtualization/Sandbox Evasion is a technique utilized by adversaries as a part of their defense evasion strategy to detect and avoid virtualization and analysis environments, such as malware analysis sandboxes.  If the malware detects a virtual machine or sandbox environment, it disengages from the victim or does not perform malicious functions, such as downloading the additional payload.

This blog explains the T1497 Virtualization/Sandbox Evasion technique of the MITRE ATT&CK®  framework, the ninth technique in the [Top 10 MITRE ATT&CK](https://top-attack-techniques.mitre-engenuity.org) techniques list.

## Why Do Attackers Use it?

Malware analysts frequently assess unknown code in isolated environments like virtual machines (VMs) or sandboxes. Similarly, security products often employ these environments to execute potentially malicious code for dynamic malware analysis before allowing it to enter the organization's network. As a result of malware analysis, TTPs (Tactic, Technique, and Procedures) used by the malware and its IOCs (Indicators of Compromise) are identified. TTPs and IOCs are used to detect the malware.

Of course, malware developers do not want their malware to be analyzed in isolated environments. Therefore, they design their code to detect virtual machine and sandbox environments and avoid exhibiting malicious behavior while running in these isolated environments. Agent Tesla remote access trojan (RAT) shuts down if it detects a sandbox environment as an example of malware sandbox evasion [2]. 

Adversaries use various methods to evade virtual machine and sandbox environments, which are referred to as "Anti-Sandbox" or "Anti-VM" methods. In general, these methods involve searching for typical characteristics of these environments. These characteristics may be some properties or objects of the victim system (e.g., a specific MAC address of a VM vendor) and the absence of common artifacts created by regular users in the system (e.g., an empty browser history).

### Sub-technique 1: T1497.001 System Checks:

Virtual machine (VM) software is intended to emulate the functionality of physical hardware. However, VM software creates artifacts indicating that it is a virtual machine rather than a physical one. Adversaries abuse this design flaw of virtual machine software and code the malware to check the system for these indicators.
Adversary Use

Sandbox evading malware uses general features that indicate a virtualization / sandbox environment to detect their operating environment. Although not all systems with these features are virtualization/sandbox environments, there is a high correlation.

  - Storage name: Hard disk drives that use names such as QEMU, VBOX, VIRTUAL HD, and VMWare.

  - HDD vendor ID: The vendor ID of the hard disc drive named VBOX or vmware. 

  - Audio device: Lack of audio device in the machine

 - Screen resolution: Low resolutions that are not frequently used in modern systems.

  -  Username: Common sandbox usernames such as sandbox, virus, malware, vmware, and test.

  -  Hostname: Common sandbox names such as cuckoo, sandbox, sample, and malware.

  -  MAC addresses: Specific MAC address prefixes 
       - 08:00:27 for VirtualBox
       - 00:05:69 for VMWare
       - 00:16:E3 for Xen
       - 00:1C:42 for Parallels

 -   Network adapter name: Specific names for network adapters (e.g., Vmware)

  -  List of directories: Certain directories such as "oracle\virtualbox guest additions\" and "VMWare".

  -  Process names: Specific processes such as vmware.exe, xenservice.exe, vmsrvc.exe, vboxservice.exe, joeboxserver.exe, and prl_cc.exe.

  -  CPU temperature: Failure or no return for CPU temperature check calls, such as MSAcpi_ThermalZoneTemperature.

  -  CPUID: The CPUID that includes the strings such as 
        - Microsoft Hv for Hyper-V
        - KVMKVMKVM for KVM
        - prl hyperv for Parallels
        - VBoxVBoxVBox for VirtualBox
        - VMwareVMware for VMWare
        - XenVMMXenVMM for Xen.


In general, malware analysts and security controls use several virtual machines in a malware analysis environment. This is because they need VMs running different versions of operating systems and software. Giving extensive resources, in terms of memory and storage sizes and processing power, to these VMs increases costs. As a result, analysts may create virtual machines with resources lower than physical machines. Adversaries use this habit and check the following system resources to understand the virtual machine environment.

  - Total physical memory size: A total RAM size that is lower than 4GB

   -  Storage size: A storage that is lower than 64 GB

  -   The number of CPU cores: A single core that is not common for modern systems.

### Sub-technique 2: T1497.002 User Activity-Based Checks:

Adversaries check past user activities to understand the environment. For example, some common artifacts usually created by users may not exist in a virtualization/sandbox environment. Adversaries also check real-time activities in the system that users regularly perform.
Adversary Use

Threat actors use the following artifacts and user activities to detect a virtual machine or sandbox environment:

-    List of files: A clean desktop or documents folder or an empty list of recent files

-    Browser usage: A short/empty browser history or cookie list

-    The number of running processes: In a regular Windows environment, at least 50 processes run simultaneously. Lower numbers may indicate a sandbox.

-    Network traffic: High uptimes (e.g., days), but low network traffic (e.g., only a few megabytes)

-    The speed/frequency of mouse movements: Infrequent mouse movements and clicks

-    Mouse clicks: Threat actors may only activate the payload when a user double clicks, such as FIN7[3]. The Okrum loader will not execute the payload until the left mouse button has been pressed at least three times [4].

### Sub-technique 3: T1497.003 Time Based Evasion

Adversaries also leverage time-based methods to detect and avoid virtualization and sandbox environments. They enumerate time-based properties to detect the environment. Since sandboxes typically analyze malware for a specified time interval, adversaries also delay execution or limit the execution period to avoid analysis in these environments.
Adversary Use

Adversaries employ various time-based methods to detect a sandbox environment.

  - Lower uptimes: Malware developers frequently use the GetTickCount() function to calculate uptime. After Windows boots, GetTickCount() begins counting. Malware can easily determine how long it has been since the computer booted up and obtain a time value for each time stamp counter cycle using this function.

  -  Delaying the execution: Adversaries delay the execution of malicious activities to evade VM or sandbox environments that are only operating for a limited period. They frequently use built-in OS commands and functions to sleep for a specified time for delayed execution. 

        -   PortDoor [5] and SUNBURST [6] backdoors and Lockfile ransomware [7] are examples of malware using the sleep command to delay malicious behavior. 
      
        -  SleepEx and NtDelayExecution are other common functions for delayed execution.
      
        - Loops and other unnecessary repetitions of commands, such as Pings, can be used to delay malware execution and potentially exceed the time limits of automated analysis environments [8]. For example, REvil ransomware used the following command for delayed execution.

ping 127.0.0.1 -n 5693 > null 

The ping command includes a -n parameter that instructs the Windows ping.exe utility to send 5,693 ICMP echo requests to localhost (127.0.0.1). This function acted as a "sleep" command, delaying the following commands by 5,693 seconds - roughly 94 minutes [9].

### References:

[1] “Virtualization/Sandbox Evasion.” https://attack.mitre.org/techniques/T1497/.

[2] S. Gallagher and M. Picado, “Agent Tesla amps up information stealing attacks,” 02Feb2021.https://news.sophos.com/enus/2021/02/02/agent-tesla-amps-up-informati on-stealing-attacks/.

[3] “FIN7 Evolution and the Phishing LNK.” https://www.mandiant.com/resources/fin7-phishing-lnk.

[4] https://www.welivesecurity.com/wp-content/uploads/2019/07/ESET_Okrum _and_Ketrican.pdf.

[5] C. Nocturnus, “PortDoor: New Chinese APT Backdoor Attack Targets Russian Defense Sector.” https://www.cybereason.com/blog/portdoor-new-chinese-apt-backdoor-at tack-targets-russian-defense-sector.

[6] “SUNBURST Additional Technical Details.” https://www.mandiant.com/resources/sunburst-additional-technical-details.

[7] “LockFile Ransomware Uses Unique Methods to Avoid Detection,” 31-Aug-2021.

https://www.esecurityplanet.com/threats/lockfile-ransomware-evasion-rechniques/.

[8] “Virtualization/Sandbox Evasion: Time Based Evasion.”

https://attack.mitre.org/techniques/T1497/003/.

[9] M. Loman, S. Gallagher, and A. Ajjan, “Independence Day: REvil uses

supply chain exploit to attack hundreds of businesses,” 04-Jul-2021. https://news.sophos.com/en-us/2021/07/04/independence-day-revil-uses- supply-chain-exploit-to-attack-hundreds-of-businesses/.

