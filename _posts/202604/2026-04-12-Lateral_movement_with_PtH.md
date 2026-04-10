---    
title:  "Lateral Movement with Pass-the-Hash" 
description: >-
     A blog on how Lateral Movement works with Pass-the-Hash (PtH) attacks in Windows environments, and how to defend against them.
date: 2026-04-12 04:30:00 +0530
categories: [GUIDE, CYBERSECURITY]
tags: [ CEH, Active Directory, Lateral Movement, Windows Security]
pin: True
---

We are taught that to log into a computer, we need a password. But computers don't actually like passwords; they like math. 

When you type `P@ssw0rd123!` into a Windows login screen, the operating system immediately converts it into a cryptographic representation called an **NTLM Hash** (e.g., `8846F7EAEE8FB117AD06BDD830B7586C`). 

Windows stores this hash, not your plaintext password. When you try to access a shared folder on another computer in the network, Windows doesn't send your password over the wire. It sends a mathematical proof based on your hash.

**The Hacker's Epiphany:** If Windows only cares about the hash, *why do I need to crack it?* Why spend weeks using a GPU to crack `8846F7EAEE8FB117AD06BDD830B7586C` back into `P@ssw0rd123!` when I can just hand the hash directly to the server and say, "Let me in"?

This is the essence of **Pass-the-Hash (PtH)**.

## The Analogy: The Bouncer and the ID


![The Bouncer and the ID](/assets/img/202604/pth.png){: .center}

Imagine a nightclub. The bouncer (Windows) doesn't ask you to recite your birth certificate (the plaintext password). He just asks to see your ID card (the Hash). 

If a hacker steals your ID card, they can show it to the bouncer. The bouncer looks at the ID, says "Looks valid to me," and opens the door. The bouncer has no way of knowing that the person holding the ID isn't the person who originally owned it.

## The Attack Flow

How does a red teamer actually execute this in an enterprise environment?

### Step 1: The Initial Compromise
You trick a user (let's say, Bob in HR) into clicking a phishing link, giving you a remote shell on Bob's workstation. You use a local privilege escalation exploit (like the SUID/Windows equivalent techniques we discussed in previous blogs) to become `SYSTEM` (the highest local admin).

### Step 2: Dumping the Secrets (Mimikatz)
Because you are `SYSTEM`, you can look into the deepest memory of Bob's computer. You use a legendary tool called **Mimikatz**.

Mimikatz looks inside a Windows process called **LSASS** (Local Security Authority Subsystem Service). This process caches the credentials of anyone who recently logged into the machine.

```text
# Running Mimikatz to extract hashes
mimikatz # sekurlsa::logonpasswords

[+] Username : IT_Admin_Alice
[+] NTLM     : 31d6cfe0d16ae931b73c59d7e0c089c0
```

*Bingo.* Yesterday, Alice from IT logged into Bob's computer to fix a printer. Her NTLM hash is still sitting in Bob's RAM.

### Step 3: Passing the Hash
You don't know Alice's password, and you don't care. Alice is a Domain Admin, meaning her hash is the master key to the entire network.

You use a tool like **CrackMapExec** or **Evil-WinRM** to pass her hash to the Domain Controller (the main server).

```bash
# Using Evil-WinRM to log in using ONLY the hash
evil-winrm -i 192.168.10.5 -u IT_Admin_Alice -H 31d6cfe0d16ae931b73c59d7e0c089c0
```

The Domain Controller accepts the hash, authenticates the session, and drops you into a remote command prompt. You have just taken over the entire corporate network without ever typing a single password.

## Defense: Breaking the Chain

Pass-the-Hash is devastating because it abuses a legitimate, built-in feature of Windows (NTLM Authentication). You can't just "patch" it, but you can architect your network to prevent it.

1.  **LAPS (Local Administrator Password Solution):** A massive issue is when every computer in a company shares the same local Administrator password. If a hacker dumps the hash on one PC, they can Pass-the-Hash to all of them. LAPS randomizes the local admin password for *every single machine*.
2.  **Tiered Administration:** Alice the Domain Admin should *never* be logging into Bob's HR computer. IT should use separate, low-privileged accounts for fixing regular workstations, and save their Domain Admin accounts strictly for logging into Domain Controllers. (This prevents high-value hashes from being left in the memory of low-security machines).
3.  **Windows Defender Credential Guard:** This is a modern Windows 10/11 feature that uses hardware virtualization to lock the LSASS process inside a secure container, making it incredibly difficult for tools like Mimikatz to extract the hashes in the first place.

## Summary for the Exam

* **Mechanism:** Authenticating to a remote server by providing the NTLM hash of a user's password, bypassing the need for the plaintext password.
* **Target:** Active Directory environments, SMB, WMI, and WinRM services.
* **Tool of Choice:** **Mimikatz** (for extraction), **CrackMapExec / PsExec** (for execution).
* **Mitigation:** Network segmentation, Tiered Admin models, LAPS, and Credential Guard.

PtH is the ultimate reminder that in cybersecurity, protecting your password isn't enough; you have to protect the math behind it.
    