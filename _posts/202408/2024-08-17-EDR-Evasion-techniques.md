---
title: EDR Evasion
description: >-
 How hackers avoid Endpoint Detection
author: anorak
date: 2024-08-17 19:55:10 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity,  Malware, Windows, Threat-Detection, Security]
pin: false
---
 EDR Evasion is a tactic widely employed by threat actors to bypass some of the most common endpoint defenses deployed by organizations. A recent study found that nearly all EDR solutions are vulnerable to at least one EDR evasion technique. In this blog, we’ll dive into 5 of the most common, newest, and threatening EDR evasion techniques as well as how you can counter them. 


## What is an EDR?
- It is a solution that records and store endpoint-system-level behaviors, use various data analytics techniques to detect suspicious system behavior, provide contextual information, block malicious activity, and provide remediation suggestions to restore affected systems.

-  While the EDR landscape is evolving, with vendors expanding their offerings to include Extended Detection and Response (XDR) and Endpoint Protection Platforms (EPP) with new features and a broader scope of coverage, it is possible that these solutions do not provide complete coverage of the attack surface.
 
-  many EDR solutions still rely on rule-based behavior finding patterns and correlations. It’s important to note that EDR technology typically does not involve the analysis of network data or metadata, this means that if a threat actor can evade the set of detection rules, the effectiveness of the EDR is immediately compromised, leaving the endpoint vulnerable to attack.

Some of the most common and useful techniques used by real malware to evade EDR detection are described below. Some of them have been active for several years but remain current and functional with the help of minor variations.



## Evasion Techniques:

### CPL ( Control Panel ) Loading:
   
  - CPL files were originally created to be associated with the Control Panel in the Microsoft Windows operating system. They are used to represent and provide quick access to various tools available in the Control Panel, making it easier to manage internal tools.
  - Malicious actors have recently been using .cpl files to hide specific (malicious software)[https://attack.mitre.org/techniques/T1218/002/] and execute it instead of legitimate system tools. Having in mind that security EDR solutions focus on identifying and blocking malicious executable files, attackers can bypass these measures by using legitimate .cpl. This technique is known as “CPL side-loading” or “CPL Hijacking”.
  - While the use of this technique may seem recent, it’s been documented since 2017 when security researchers discovered that the FIN7 cybercrime group was using it to attack financial institutions. Since then, numerous attacks have been identified that employ CPL side-loading as part of the cyber kill chain.
  - The CPL side-loading technique is relatively easy to perform, as there are several tools available, such as (CPLResourceRunner)[https://github.com/rvrsh3ll/CPLResourceRunner] and (ScareCrow)[https://github.com/optiv/ScareCrow], that allow for the creation of a malicious embedded CPL file. This seemingly simple technique can be combined with other techniques, such as the use of memory-mapped files (MMFs) to store shellcode, in order to prevent other EDR functionalities from detecting anomalous behavior.
  - The study mentioned earlier found that **48% of EDR** and EPP solutions tested are not able to detect or block the execution of this technique immediately, which increases the risk of compromise.
    
                               
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              
###  DLL Side Loading:

- This (technique)[https://attack.mitre.org/techniques/T1574/002/] is used by attackers to run malicious code by deceiving an application into loading a malicious DLL (Dynamic Link Library) file instead of a genuine one. In other words, the attackers replace a legitimate DLL with their malicious one and make the application load it, which results in the execution of the attacker’s code on the target system.
- A DLL, or Dynamic Link Library, is a type of software library that contains code and data which can be shared and used by multiple programs simultaneously. It works like a shared toolset that different programs can use to accomplish certain tasks.
- For example, in Windows Comdlg32 DLL is a commonly used library that provides a functionality for creating dialog boxes. This means that any program that needs to create a dialog box can use the code and data stored in the Comdlg32 DLL, rather than having to create the dialog box from scratch.
- Side-loading is a technique similar to DLL Search Order Hijacking, where an attacker hijacks the DLL a program loads. While side-loading contains hijacking, attackers don’t just add the malicious DLL to the search order of a program and wait for the victim application to be launched. Instead, attackers directly side-load their payloads by placing and running their malicious code through a legitimate application. That is to say, they exploit a trusted application to execute their malicious code, rather than waiting for the victim application to call it.
- To perform a successful DLL side-load, an attacker must deceive a Windows application into loading a malicious DLL file exploiting the known DLL search order of Microsoft applications, which is a relatively straightforward process.
- The default search order for Microsoft application DLLs varies depending on whether DLL safe search mode is enabled or disabled. When safe DLL search is enabled, the application will search for the required DLL files in the following order,
    - The directory where it was loaded, 
    - The system directory, 
    - The 16-bit system directory, 
    - The Windows directory, 
    - The current directory, 
    - Any directory listed in the PATH Environmental variable. 
- When DLL safe search mode is disabled, the search order is different, with the application first looking for:

    - The directory where it was loaded
    - The current directory, 
    - The system directory, 
    - The 16-bit directory
    - Any directory listed in the PATH environment variable.
- When safe DLL search mode is enabled, the current directory where a user executes a program is given less priority in the search order for DLL files. However, when safe search is disabled, the current directory is slightly higher in the hierarchy, making it more likely for an application to search for a DLL file in that directory.
- Attackers can take advantage of this default search order by placing a malicious DLL file in a directory that an application will search before it finds the legitimate DLL. In effect they are tricking the application into loading the malicious DLL instead of the legitimate one, leading to a DLL hijacking attack.
- The study shows that approximately **68% of all EDR and EPP technologies** are vulnerable to this kind of attack.

### Code Injection:

- (Code injection)[https://attack.mitre.org/techniques/T1055/] is a method employed by attackers to introduce malicious code into a legitimate application or process in order to bypass detection by EDR or EPP systems. This method of executing arbitrary code in the address space of another live process can mask its presence under a legitimate process, making it harder for security products to detect.
- CreateRemoteThread() and QueueUserAPC() are two commonly used Windows API functions for code injection. The latter was used in the mentioned study, which involves queuing a user-mode APC to another thread. An APC is a function executed in the context of the target thread when it enters an alertable state. This technique is also referred to as “Early Bird”.
- While new code injection techniques have emerged, older ones such as process hollowing remain a popular choice for attackers and are still successful even today. In this technique the attacker must create a new process in a suspended state, for this task he uses the CreateProcess() function of the Windows API. The process then “Hollows out” itself by deallocating the memory pages of the legitimate binary from the new process’s address space using the ZwUnmapViewOfSection() or NtUnmapViewOfSection Windows API functions. This leaves the new process with an empty address space.

- In addition, the attacker allocates memory in the address space of the new process using the Windows API function VirtualAllocEx(). After, the malicious code is copied into memory allocated by the Windows API function WriteProcessMemory(). The attacker then modifies the execution context of the new process by setting the entry point to the address of the malicious code.

- Finally, the attacker resumes execution of the new process using the ResumeThread() function of the Windows API. The new process will now execute the malicious code instead of the legitimate code.

- One of the most recent techniques for code injection is “Atomic Bombing”, this technique takes advantage of the Windows mechanism for interprocess communication (IPC) called “atoms.” An atom is a unique identifier for a string or data that is stored in a global table called an atom table. Each atom has a unique 16-bit identifier, which is used to retrieve the corresponding string or data value.

- The attacker then sends an Asynchronous Procedure Call (APC) to the APC queue of a target process thread using the NtQueueApcThread() API. When APC runs, it triggers the target process to call the GlobalGetAtomName function, which retrieves the malicious code from the global atomic table and inserts it into the target process’s memory.

- While this technique was an innovative way to write malicious code into the memory space of another process without using WriteProcessMemory, the inserted code is still not executable. An additional step is needed to make the code executable, which involves using another APC call to run Return-Oriented Programming (ROP) and convert the memory region of the code into readable, writable, executable (RWX) memory. Only after this step is the malicious code ready to run.

- The study mentioned above used the “Early Bird” technique for code injection and the result shows that approximately  **65% of EDR and EPP technologies** allow the execution of this method without blocking it.


















    
