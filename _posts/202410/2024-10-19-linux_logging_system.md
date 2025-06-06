---
title: Linux For beginners 02
description: >-
  A basic article about linux logging system for beginners
author: anorak
date: 2024-10-19 00:30:00 +0530
categories: [GUIDE,LINUX]
tags: [Linux,  Security]
pin: true
---

One of the most overlooked areas on the Linux operating system is the logging system. If you are a system administrator or security engineer, this is where all the information you will need resides to determine the problems with your operating system, including who, what and why of your intrusion. If you are a penetration tester/hacker, this is where the evidence of your intrusion resides. As the best hack is the one that no one is aware of, it becomes critical to remove any trace of your presence from the log files. To do so, you first need to understand the logging system. With the adoption of systemd in most Linux distributions, the logging system has changed from syslog. In most cases, your Linux is using journalctl to manage your system logs.


## Journalctl:The Hacker's Guide to Linux Logging

As a cyberwarrior, understanding and manipulating system logs is a critical skill in your arsenal. In the realm of modern Linux systems, journalctl stands out as a powerful tool for centralized log management. This article will delve deep into the intricacies of journalctl, exploring its potential from an offensive security standpoint. We'll cover how to use it effectively, its strengths and weaknesses, and how it can impact your cyberwarrior/red team operations.

## What is journalctl? 

journalctl is the query tool for systemd's journal, a centralized logging system in many modern Linux distributions. It collects and stores logging data from various sources, including the kernel, system services, and applications.


### Benefits of journalctl:

   1. Centralized Log Access: journalctl provides a single point of access for nearly all system logs. This centralization can be a double-edged sword for an attacker. On one hand, it simplifies log analysis and manipulation. On the other, it means you need to be extra vigilant about leaving traces.

   2. Powerful Filtering Capabilities: journalctl offers extensive filtering options, allowing you to pinpoint specific information quickly. This is invaluable when you're analyzing system behavior or cover your tracks.

   3. Real-time Monitoring: The ability to watch logs in real-time can provide immediate feedback on your actions, allowing  you adjust your tactics on the fly.

   4. Structured Data: journalctl can output logs in various formats, including JSON, which makes programmatic analysis much easier.

   5. Boot-specific Analysis: You can easily isolate logs from specific system boots, which is crucial for understanding system changes after a reboot or when analyzing the impact of your persistence mechanisms.


### Drawbacks of journalctl:


 1.   Increased Logging: The comprehensive nature of journald means more of your actions may be logged compared to traditional syslog systems.

 2.   Tamper Evidence: journald uses cryptographic sealing of log files, making direct tampering more difficult and potentially more detectable.

 3.   Remote Logging: Many systems are configured to forward journald logs to remote servers, which can complicate log manipulation efforts.

 4.   Resource Usage: In some cases, extensive logging can impact system performance, potentially making your presence more noticeable.

## Journalctl usage scenario:

Imagine you're a cyberwarrior hacking a large corporation's internal network. You've successfully gained initial access to a Linux server through a vulnerable web application. Your goal is to escalate privileges, gather intelligence, and potentially pivot to other systems while evading detection.

### Step 1: Initial Reconnaissance:

After gaining a shell on the compromised server, one of your first moves is to use journalctl to gather system intelligence:

``` target> journalctl -q --since "24 hours ago" ```

![img](/assets/img/202410/log1.webp){: width="600" height="600" .center}

This command quietly retrieves the last 24 hours of logs. The -q flag suppresses informational messages, reducing the noise and potential traces of your activities.

### Step 2: Investigating User Activities:

To understand recent user activities, you run:

```target> journalctl _UID=1000 --since "24 hours ago" ```
![img](/assets/img/202410/log2.webp){: width="600" height="600" .center}


This reveals that user "air" (UID 1000) has been actively using the system and has used “sudo” several times.

### Step 3: Examining Service Behavior:

You can investigate, for example, the Apache web server service and send the output in json-pretty format (more human friendly):

```target> journalctl -u apache2 -o json-pretty```
![img](/assets/img/202410/log3.webp){: width="600" height="600" .center}

The JSON output allows you to quickly parse the logs.

### Step 4: Kernel-level Analysis:

To check for any unusual kernel activities:

```target> journalctl -k --since "24 hours ago"```
![img](/assets/img/202410/log4.webp){: width="600" height="600" .center}

### Step 5: Privilege Escalation Attempt:

Based on the sudo usage patterns observed, you might attempt a privilege escalation exploit. To monitor its effects in real-time:

```target> journalctl -f -p err..emerg ```
![img](/assets/img/202410/log5.webp){: width="600" height="600" .center}

This allows you to watch for any high-priority error messages that your exploit might trigger.

### Step 6: Covering  Your Tracks:

After successfully escalating privileges, you need to clean up traces of your activities. Instead of deleting logs (which might alert defenders), you use:

```target> journalctl –vacuum-time=2d```
![img](/assets/img/202410/log6.webp){: width="600" height="600" .center}

This removes logs older than two days, potentially erasing evidence of your initial access, while appearing less suspicious than targeted log deletion.

### Step 7: Persistence Mechanism:


To establish persistence, you create a systemd service that masquerades as a legitimate logging process. You monitor its behavior using:

```target> journalctl -f -u your-fake-service```

This allows you to ensure your persistence mechanism is functioning without raising alarms.

### Step 8: Lateral Movement:

As you prepare to pivot, for example, to the database server, you can set up ongoing monitoring:

```target> journalctl -f | grep -E '<IP>|corpApp' ```

This helps you understand the normal communication patterns between the compromised server and the database, allowing you to time your lateral movement attempts to blend in with regular traffic.

### Usage and Analysis:

 1.   Stealth: Throughout the operation, the "-q" option is used to suppress additional journalctl output, reducing the chance of alerting system administrators to unusual log querying activity.

 2.   Service Analysis: Investigating specific services like "corpApp" provides valuable intelligence about the target environment's architecture and potential pivot points.

 3.   Real-time Monitoring: The "-f" option is crucial for providing immediate feedback on your actions, allowing for quick adjustments to your tactics if unexpected logs are generated.

 4.   Log Manipulation: Instead of crude log deletion, using the "--vacuum-time" option appears more like routine system maintenance, reducing suspicion.

 5.   Lateral Movement Intelligence: By monitoring logs related to inter-server communication, you can better mimic legitimate traffic patterns during pivoting attempts.

### Summary

journalctl is a powerful tool that presents both opportunities and challenges for cyberwarriors. Its centralized nature and powerful querying capabilities make it an invaluable resource for system intelligence gathering and log manipulation. However, its comprehensive logging and potential for remote forwarding also increase the risk of detection.

As with all tools in your hacking toolkit, the key to effectively leveraging journalctl lies in understanding its capabilities and limitations. Use it wisely to enhance your operations, but always be mindful of the traces you may leave behind.
*






















