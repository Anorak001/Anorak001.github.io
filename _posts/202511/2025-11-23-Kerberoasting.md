---
title: " A Deep Dive into Kerberoasting: OSCP Edition "
description: >-
        A concise guide to Kerberoasting, a powerful technique for compromising AD service accounts.
author: anorak
date: 2025-11-23 03:10:00 +0530
categories: [ GUIDE, CYBERSECURITY]
tags: [Cybersecurity, Security,   Kerberoasting, OSCP, Active-Directory ]
pin: True
---

## Introduction: 

If you are preparing for the **OSCP** (or the updated **OSCP+**), you know the reality: **40 points** of your exam depend on compromising an Active Directory set. You usually start with an "Assumed Breach" scenario—access to a low-privilege user account. What is your first move?

Enter **Kerberoasting**.

It is the closest thing to a "magic bullet" in Windows environments. It requires **no special privileges**, triggers **minimal alerts** (if executed correctly), and allows you to crack passwords **offline** on your own hardware, far away from the Blue Team's monitoring tools.

> **The "Too Long; Didn't Read":**
>
>   * **Target:** Service Accounts (users that run services like SQL, IIS, etc.).
>   * **Goal:** Steal their Kerberos Service Ticket (TGS).
>   * **Win:** Crack the ticket offline to reveal the plaintext password.

 

## The Theory: How It Works

To exploit it, you must understand the underlying mechanism. Kerberos acts as the gatekeeper for Windows Active Directory.

1.  **The SPN (Service Principal Name):** In AD, every service (like an MSSQL server) needs a unique identifier so Kerberos can locate it. This is the SPN.
2.  **The Request (TGS-REQ):** Any valid user in the domain can ask the Domain Controller (DC): "I want to communicate with the SQL Service. Please issue me a ticket."
3.  **The Response (TGS-REP):** The DC verifies the service exists. If it does, it encrypts a ticket using the **Service Account's NTLM hash** and delivers it to the requestor.
4.  **The Flaw:** The DC **does not check if you have permission to access the service** before issuing the ticket. It simply hands it over because the service itself is responsible for the final access check.

![ Kerberos Authentication protocol ](/assets/img/202511/Kerberos-authentication-protocol.jpeg){: .center}

**The Exploit:** You capture that encrypted ticket. Since it is encrypted with the service account's password hash, you can brute-force it. If you guess the password that successfully decrypts the ticket, **you have compromised the account.**

 

## The Attack Lifecycle (OSCP Methodology)

We will look at two primary methods: **Python (Linux/Kali)** and **C\# (Windows/native)**.

### Phase 1: Enumeration & Extraction

You have established a shell or possess credentials. Now, identify the targets.

#### Option A: The "Impacket" Way (From Kali)

If you have credentials (username/password) but no shell, or you are pivoting via a SOCKS proxy, this is the standard approach.

```bash
# Syntax: GetUserSPNs.py domain/user:password -request
GetUserSPNs.py megacorp.local/lowpriv:Welcome123 -dc-ip 192.168.10.5 -request
```

  * **Mechanism:** It queries the DC for all users with an SPN associated with them and dumps the hash immediately.
  * **Why use it:** This is arguably the most stable method for the OSCP exam as it runs entirely from your attacking machine.

#### Option B: The "Rubeus" Way (From Windows)

If you have a shell on a compromised Windows machine, use **Rubeus**. It is generally faster and offers better operational security (OPSEC).

```powershell
# Compile Rubeus.exe and upload it
.\Rubeus.exe kerberoast /nowrap
```

  * **`/nowrap`**: This flag is essential. It ensures the hash is printed on a single line, preventing copy-paste errors.
  * **`/stats`**: This allows you to see how many "roastable" accounts exist without actually performing the attack, keeping noise to a minimum.

 

### Phase 2: Cracking the Hash (The "Offline" Phase)

You now possess a text blob that resembles `$krb5tgs$23$*...`. This is the target data.

Save this string into a file named `hashes.txt` on your **host machine**. Do not attempt to crack this inside the exam VM; utilize your host hardware (GPU) for maximum speed.

#### Using Hashcat

Hashcat mode `13100` is designed for Kerberos 5 TGS-REP etype 23 (the standard format).

```bash
# Standard RockYou attack with rule mutation
hashcat -m 13100 hashes.txt /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```

  * **Why use rules?** The `best64.rule` argument will attempt variations of the passwords in your wordlist (e.g., appending "123", changing case, or adding "\!" to the end). This is critical for passing OSCP, as service accounts often follow patterns like `ServiceName!2024`.

 

## Defense (For the Experts)

Understanding the mitigation is just as important as the attack.

  * **The Problem:** Service accounts are often configured with "Password Never Expires" and are rarely rotated because changing them can break production applications.
  * **The Mitigation:**
      * **AES Encryption:** Force AES encryption for Kerberos. It is significantly more computationally expensive to crack than RC4.
      * **Managed Service Accounts (gMSA):** These are special AD accounts where Windows manages the password automatically. They use 120+ character complex passwords that are mathematically impossible to brute force.
      * **Honeytokens:** Create a fake SPN account that is never used. Configure monitoring to alert immediately if anyone requests a ticket for it. This is a high-fidelity indicator of compromise.

 

## OSCP Exam Checklist

When you encounter the Active Directory set in your exam:

1.  **Check Immediately:** The moment you obtain valid user credentials, run `GetUserSPNs.py`. It takes seconds and could potentially yield Domain Admin privileges if a high-level account is vulnerable.
2.  **Time Management:** Begin the cracking process immediately in the background while you continue manual enumeration. Do not wait for Hashcat to finish before moving on.
3.  **Check Description Fields:** Occasionally, the service account description in AD (visible via BloodHound or LDAP) contains hints about the password or its purpose.

 

### Final Thoughts

Kerberoasting is a race against entropy. It exploits the human element—weak passwords on accounts that are technically required to be exposed to the network. Master this technique, and you master the first step of Active Directory domination.

**Happy Hacking.**

 