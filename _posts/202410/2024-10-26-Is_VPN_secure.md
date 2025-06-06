---
title: Secure VPN 
description: >-
  Is VPN really safe as much as you think it is?
author: anorak
date: 2024-10-26 00:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [ Security, Vulnerabilities, Cybersecurity, CVE,  Network Security, Serious Issue]
pin: false
---

So, You thought VPN was keeping you secure??

Although VPN's are sold to keep you safe and secure online, many of them are from from safe and secure. In fact, some of them may actually make you less secure. This is because the developers, in many cases, did not take the most rudimentary steps toward securing these devices.

the Russians took down the ViaSat satellite internet system in Ukraine and Europe by exploiting a Fortinet VPN vulnerability where the attacker could find the cleartext credentials using a simple directory traversal attack (../../).


## VPN's in Cybersecurity:


In the fast-changing world of cybersecurity, Virtual Private Networks (VPNs) have been touted as essential tools for organizations of all sizes and consumers. They provide a relatively safe way for employees to connect to company networks remotely, allowing for flexible work arrangements while safeguarding sensitive information. However, despite their importance, VPNs can also be potential weak points that pose significant risks.

In 2023, the cybersecurity landscape faced a startling reality: there was a dramatic 47% increase in reported VPN vulnerabilities compared to 2022. This alarming statistic has raised significant concerns within the industry, prompting organizations to reevaluate these security measures.

While VPNs are critical for ensuring secure remote access and protecting sensitive data from interception, the rise in vulnerabilities poses a serious threat. If attackers manage to exploit these weaknesses, they can infiltrate networks without detection. This could lead to serious data breaches, ransomware attacks, or extortion. APT around the world have been exploiting these vulnerabilities to access credentials and what were thought of as secure communication.

Given these risks, organizations must understand the vulnerabilities associated with their VPNs. This article will explore the top 5 VPN vulnerabilities identified from 2022 through mid-2024.

## CVE-2024-24919 (Information Disclosure in Check Point Quantum Gateway/Spark Gateway/CloudGuard Network Remote Access VPN)

CVE-2024-24919 is an information disclosure vulnerability that can allow an attacker to access certain information on internet-connected Gateways that have been configured with IPSec VPN, remote access VPN or mobile access software blade. According to the advisory, Check Point has observed in-the-wild exploitation of this vulnerability and so far this exploit activity has been focused on devices configured with local accounts using password-only authentication.

## CVE-2024-21762 (Out-of-Bounds Write in Fortinet FortiOS)


CVE-2024-21762 is an out-of-bounds write vulnerability in sslvpnd, the SSL VPN daemon of Fortinet's  FortiOS. An unauthenticated attacker could exploit this vulnerability by sending specially crafted HTTP requests to a vulnerable device with SSL VPN enabled. Successful exploitation would enable the attacker to execute remote code (rce) or commands on the device.

Almost a month after Fortinet addressed CVE-2024-21762, Shadowserver Foundation reported the discovery of nearly 150,000 vulnerable devices.

## CVE-2024-3400 (Command Injection in Palo Alto Networks PAN-OS GlobalProtect)

CVE-2024-3400 is a critical command injection vulnerability affecting the GlobalProtect Gateway feature of PAN-OS. An unauthenticated remote attacker could exploit this vulnerability to execute code on an affected firewall with root privileges!

This vulnerability impacts specific versions of PAN-OS, including 10.2, 11.0, and 11.1. Palo Alto Networks has confirmed that several attacks have exploited this vulnerability since its disclosure. Detailed analysis has revealed the sophistication and intent of the attackers, who are linked to the state-sponsored threat actor UTA0218. These attackers utilized the vulnerability to establish a reverse shell, enabling them to download malicious tools, exfiltrate sensitive configuration data, and conduct lateral movement within compromised networks.

## CVE-2023-27997 (Heap-Based Overflow in Fortinet FortiOS/FortiProxy FortiGate SSL-VPN)

CVE-2023-27997 is a heap-based buffer overflow vulnerability affecting the secure socket layer virtual private network (SSL VPN) functionality in FortiOS and FortiProxy on Fortinet devices, including FortiGate Next Generation Firewalls (NGFW). An unauthenticated remote attacker can exploit this vulnerability by sending specially crafted requests to a vulnerable device, potentially allowing the attacker to execute arbitrary code on the affected device.

This disclosure mirrors previous vulnerabilities reported by Fortinet, such as CVE-2022-40684, a critical authentication bypass in FortiOS and FortiProxy, and CVE-2022-42475, another heap-based buffer overflow in FortiOS SSL VPNs. Fortinet has had a very bad few years. I'm sure why anyone still trusts their products.

CVE-2022-42475 was exploited in the wild by suspected Chinese threat actors, resulting in the compromise of a government entity in Europe and a managed service provider in Africa. Furthermore, Fortinet's recent blog post indicates that CVE-2022-40684 was targeted by a newly identified threat actor known as Volt Typhoon.

## CVE-2023-46805 & CVE-2024-21887 (Improper Authentication & Command Injection in Ivanti Connect Secure/Policy Secure Web)

CVE-2023-46805 and CVE-2024-21887 are two critical vulnerabilities that have had a significant impact on Ivanti Connect Secure (ICS) and Ivanti Policy Secure (IPS) products. These flaws have drawn considerable attention from cybersecurity professionals due to their severity and potential implications.

The first vulnerability, CVE-2023-46805, is an authentication bypass within the appliances' web component. This allows attackers to access restricted resources by bypassing essential control checks. The second vulnerability, tracked as CVE-2024-21887, is a command injection flaw that enables authenticated administrators to execute arbitrary commands on vulnerable appliances through specially crafted requests.

Ivanti has reported that these two zero-day vulnerabilities have already been exploited in the wild, affecting a limited number of customers. Threat intelligence firm Volexity identified the exploitation of these zero-days in December, linking the attack to a suspected Chinese state-backed threat actor.

This isn’t the first time that Chinese threat groups have targeted Ivanti products; in 2021, they exploited another zero-day in Connect Secure, tracked as CVE-2021-22893, which led to breaches affecting numerous U.S. and European government, defense, and financial organizations.

## Summary:


VPN's are sold as products that will keep us secure. Unfortunately, lax security standards at these developers have made many of these devices less than secure. This means that if you are dependent upon them to keep you safe and secure, your trust is probably misplaced

















