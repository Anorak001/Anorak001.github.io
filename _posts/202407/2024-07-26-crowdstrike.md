---
title: Crowdstrike Outage Explained
description: >-
 What caused one of the biggest IT outages the world has ever faced
author: anorak
date: 2024-07-25 00:35:00 +0530
categories: [NEWS,ISSUE]
tags: [Serious Issue,Cybersecurity]
pin: false
---
What might be considered the **largest IT outage** in history was triggered by a botched software update from security vendor CrowdStrike, affecting millions of Windows systems around the world. Insurers estimate the outage will cost U.S. Fortune 500 companies  **$ 5.4 B**.

 
## The Issue:

- The outage occurred **July 19, 2024**, with millions of Windows systems failing and showing the infamous blue screen of death (BSOD).
- CrowdStrike, the company at the core of the outage, is an endpoint security vendor whose primary technology is the Falcon platform, which helps protect systems against potential threats in a bid to minimize cybersecurity risks.

- In many respects, the outage was a real manifestation of fears that computing users had at the end of the last century with the Y2K bug. With Y2K, the fear was that a bug in software systems would trigger widespread technology failures. 
- While the CrowdStrike failure was not Y2K, it was a software issue that did, in fact, trigger massive disruption on a scale that has not been seen before.
  
***

### What caused the outage?

![Crowdstrike Falcon](/assets/img/202407/crowdstrike.jpeg){: width="800" height="400" .w-50 .right}

- The outage was not a Microsoft Windows flaw directly, but rather a flaw in CrowdStrike Falcon that triggered the issue.
  
- CrowdStrike Falcon is a cloud-based endpoint protection platform developed by CrowdStrike. It is designed to detect, prevent, and respond to a wide range of cybersecurity threats and attacks.
  
- Falcon hooks into the Microsoft Windows OS as a Windows kernel process. The process has high privileges, giving Falcon the ability to monitor operations in real time across the OS.
  There was a logic flaw in Falcon sensor version 7.11 and above, causing it to crash.
  Due to CrowdStrike Falcon's tight integration into the Microsoft Windows kernel, it resulted in a Windows system crash and BSOD.
  
- The flaw in CrowdStrike Falcon was inside of a **sensor configuration update**. The sensor is regularly updated -- sometimes multiple times daily -- to provide users with mitigation and threat protection.
  
- The flawed update was contained in a file that CrowdStrike refers to as "channel files," which specifically provide configuration updates for behavioral protections. Channel file 291 is an update that was supposed to help improve how Falcon evaluates named pipe execution on Microsoft Windows. Named pipes are a common type of communication mechanism for interprocess communications on Microsoft Windows.
  
  With channel file 291, CrowdStrike inadvertently introduced a logic error, causing the Falcon sensor to crash and, subsequently, Windows systems in which it was integrated.

- The flaw isn't in all versions of channel file 291. The problematic version is channel file 291 (**C-00000291*.sys**) with timestamp 2024-07-19 0409 UTC (Its FRIDAY!!). Channel file 291 timestamped **2024-07-19 0527 UTC** or later does not have the logic flaw.
  
- By that time, CrowdStrike had noticed its error and reverted the change. But, for many of its users, that reversion came too late as they had already updated, leading to BSOD and inoperable systems.
  
- [Here}(https://www.crowdstrike.com/falcon-content-update-remediation-and-guidance-hub) is the detailed troubshooting steps for fixing the Windows BSOD.

  ***

### Why Apple and Linux were not affected:

- CrowdStrike's software doesn't just run on Microsoft Windows; it also runs on Apple's macOS and the Linux OS.But the July outage only affected Microsoft Windows. The root cause of the outage was a faulty sensor configuration update that specifically affected Windows systems. The channel file 291 update was never issued to macOS or Linux systems as the update deals with named pipe execution that only occurs on the Microsoft Windows OS.

- The way that the Falcon sensor integrates as a Windows kernel process is also not the same in macOS or Linux. Those OSes have different integration points to limit potential risk.

![Linux Users](/assets/img/202407/linux_meme.webp){: width="800" height="400" .w-50 .right}

- However, there was a [reported incident](https://access.redhat.com/solutions/7068083) in June from Linux vendor Red Hat, where the Falcon sensor, running as an eBPF program in Linux, triggered a kernel panic. In Linux, a kernel panic is a type of crash, though typically not as dramatic as BSOD. That issue was resolved without Red Hat reporting any major incidents.


***

## What services were affected?

- Microsoft estimated that approximately 8.5 million Windows devices were directly affected by the CrowdStrike logic error flaw. That's less than 1% of Microsoft's global Windows install base.

But, despite the small percentage of the overall Windows install base, the systems affected were those running critical operations. Services affected include the following.

![Airport_India](/assets/img/202407/crowdstrike_outage.webp){: width="600" height="400" .w-50 .right}

  1. Healthcare
  2. Airline and Airports
  3. Financial Services
  4. Media and Broadcasting


***

## Hackers take advantage of the Outage:
- While the outage was not due to a cyberattack, threat actors have taken advantage of the incident.
  According to a [blog](https://www.crowdstrike.com/blog/falcon-sensor-issue-use-to-target-crowdstrike-customers) post from CrowdStrike, the security vendor has received reports of the following malicious activity:

  - Phishing emails sent to customers posing as CrowdStrike support.
  - Fake phone calls impersonating CrowdStrike staff.
  - Selling scripts claiming to automate recovery from the botched update.
  - Posing as independent researchers saying the outage was due to a cyberattack and offering remediation insights.

***

## Final Thoughts:

- Though the update which contained the bug was quickly installed within seconds,now the issue has to be fixed by deleting the file 291 (C-00000291*.sys) manually and troubleshooting it.This is where an IT worker has to manually go and fix the device individually which is a time consuming task.
  
- Some businesses were able to apply the fix within a few days. However, the process was not straightforward for all, particularly those with extensive IT infrastructure and encrypted drives. The use of the Microsoft Windows BitLocker encryption technology by some organizations made it significantly more time-consuming to recover as BitLocker recovery keys were required.
  
- It is estimated that it could potentially take months for some organizations to entirely recover all affected systems from the outage.
 









