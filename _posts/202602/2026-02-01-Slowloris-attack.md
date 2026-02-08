---
title: " The Slow Death: Taking Down Servers with Slowloris "
description: >-
        Deep Dive into the Slowloris Denial-of-Service Attack and Its Mitigation Strategies
author: anorak
date: 2026-02-01 03:10:00 +0530
categories: [ GUIDE, CYBERSECURITY]
tags: [Cybersecurity, CEH, Attack] 
pin: false
---  
 

## Introduction: The "Waiter" Problem

Imagine a restaurant with only 50 tables.
In a traditional **Flood Attack** (like a DDoS), the attacker sends 10,000 fake customers to storm the front door at once. The security guard (Firewall) sees the mob and locks the door. The attack is noisy and easy to block.

**Slowloris** is different.
The attacker sends 50 guys into the restaurant. They sit down at every single table. When the waiter asks for their order, they say:
*"I'll have a water... and... um... let me think about the soup... for 10 minutes."*

They never finish their order. They never leave.
The restaurant is technically "open," but no real customers can get a table. The service grinds to a halt without a single alarm going off.

![Slowloris Attack Illustration](/assets/img/202602/slowrois.png){: .center}

## The Technical Flaw: Thread Exhaustion

Web servers (like Apache) are designed to handle multiple connections at once. However, they have a limit (e.g., `MaxClients = 150`).

When you connect to a website, your browser sends an HTTP Request:

```http
GET / HTTP/1.1
Host: www.target.com
User-Agent: Mozilla/5.0
Accept: text/html
[CRLF] [CRLF]  <-- The "Double Enter" means "I am done."

```

The server waits for that final `[CRLF]` (Carriage Return Line Feed) to know the request is complete so it can send the webpage.

**The Attack:**
Slowloris connects, sends the header, but **never sends the final CRLF**.
It sends:
`GET / HTTP/1.1`
`Host: target.com`
... (Wait 10 seconds) ...
`X-Random-Header: 1`
... (Wait 10 seconds) ...
`X-Random-Header: 2`

The server keeps the connection open, waiting for the rest of the request. The attacker opens 150 of these connections, using up every available slot. The CPU usage is near zero, bandwidth usage is tiny, but the site is completely dead to the outside world.

## The Tool: Slowloris.py

This script is incredibly simple (originally written in Perl by RSnake, now commonly Python).

```bash
# Basic usage targeting a web server
python3 slowloris.py target.com -s 200

```

* `-s 200`: Open 200 sockets (connections).

You will see the output:
`[+] Sending keep-alive headers...`
`[+] Sending keep-alive headers...`

If you try to visit `target.com` in your browser while this is running, it will just spin forever.

## Why Firewalls Fail

Traditional Firewalls look for **volume**.

* "Is this IP sending 1000 packets per second?" -> **BLOCK.**

Slowloris sends maybe **1 packet every 15 seconds**.
To a firewall, this looks like a user on a very slow mobile connection. Blocking it is difficult without blocking legitimate users with bad internet.

## The Defense: Architecture Changes

You cannot "patch" this; it's a flaw in how connection-based servers work. You must change the architecture.

1. **Reverse Proxies (Nginx/HAProxy):**
Unlike Apache, Nginx uses an "Event-Driven" architecture. It can handle thousands of slow connections without sweating. Putting Nginx *in front* of Apache protects it. Nginx will wait for the *whole* request before passing it to Apache. The Slowloris attack gets stuck at Nginx, which just shrugs it off.
2. **Timeouts:**
Configure the server to drop connections that take too long to send headers (e.g., `ModSecurity` or `mod_reqtimeout` in Apache).
3. **Cloudflare:**
Modern CDNs buffer requests. They won't send the request to your real server until it is complete.

## Summary for the Exam

* **Type:** Application Layer DoS (Layer 7).
* **Target:** Thread-based web servers (Apache 1.x/2.x).
* **Mechanism:** Sending partial HTTP requests and keeping sockets open with "keep-alive" packets.
* **Signature:** Low bandwidth, low CPU, but high "Open Connections" count.
 