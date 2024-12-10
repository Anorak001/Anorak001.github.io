---
title: Session Hijacking 101
description: >-
 Article on how Cybercriminals Take Over Your Online Sessions
author: anorak
date: 2024-10-06 14:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity]
pin: true
---
According to the Verizon Data Breach report from 2023, over the past 25 years one hacker threat has remained the number one attack vector – phishing.  Phishing is ubiquitous and deadly.  Phishing attacks today are more impactful, sophisticated, and frequent than ever before in history.  Recent developments in hacker techniques have made phishing emails even more effective through the use of session token theft, aka Session Hijacking.  This is a sophisticated way for attackers to take over your online accounts post authentication (i.e. after you have successfully provided your password and multi-factor authentication).

Session hijacking is being combined with advanced threats like AiTM (Adversary-in-the-Middle), BiTM (Browser-in-the-Middle) attacks making it even more dangerous. Let’s break down how these attacks work and, more importantly, what you can do to protect yourself.


## What is Session Hijacking?

When you log into a website, whether it’s your email, bank account, or social media, a “session” is created between your browser and the site. A unique token, called a session token, is issued to your browser to confirm your identity for the duration of your visit. If attackers steal this token, they can impersonate you online—gaining access to your account without knowing your password or needing access to your multi-factor authentication service.

## How AiTM and BiTM Make Session Hijacking More Dangerous:

### Adversary-in-the-Middle (AiTM):

AiTM attacks happen when hackers insert themselves between you and the legitimate website you’re trying to access. Even if you’re using strong passwords and multi-factor authentication (MFA), AiTM attacks can bypass these defenses by intercepting your session tokens and capturing the entire session. This type of attack was detailed in a recent CyberHoot Blog article on Fake Airplane Wi-Fi. This attack allows hackers to essentially impersonate you, accessing your accounts with full privileges.

### Browser-in-the-Middle (BiTM):

BiTM is a similar technique, but it focuses on exploiting vulnerabilities in your browser. Malicious code or extensions can be injected into your browser, allowing attackers to hijack your session tokens without your knowledge. BiTM attacks take advantage of outdated or insecure browsers to steal session tokens directly from your device.  All they require is you to click a malicious link or visit a malicious website that reaches into your browser to steal these active email and banking session tokens

### Infostealers:

Infostealers are malware designed to quietly collect sensitive information from infected devices. Once installed, these programs can steal login credentials, session cookies, and other personal data stored in your browser. Infostealers can also send this information back to the attacker in real-time, allowing them to hijack your session or use your stolen credentials immediately.

## The Consequences of Session Hijacking:

When attackers gain access to your session, they have full control over your account without needing to guess your password and intercept your MFA code. With session hijacking hackers can:
### Steal personal data:

  -  Attackers can access sensitive information, including banking information, all personal or professional email messages, and potentially accessing online file storage solutions (DropBox, Egnyte etc.).

### Commit financial fraud:

   - With access to accounts like online banking or e-commerce, cybercriminals can initiate unauthorized transactions or make purchases in your name.  This is one reason why banks require multifactor authentication to add new payees or initial large funds transfers.

### Perform identity theft:

   - Attackers can use your online identity to take out financial instruments in your name (Credit Cards, Loans etc), causing long-term damage to your finances.

## How You Can Protect Yourself

Given the growing sophistication of these attacks, it’s crucial to be proactive about securing your accounts and protecting your sessions. Here’s what you can do:

1. Awareness of AiTM and BiTM Attacks:
Understand that even with strong passwords and MFA, you are still vulnerable to AiTM and BiTM attacks if attackers intercept your session token. Use a VPN when connecting to public or unsecured networks.  Always keep an eye out for suspicious activity on your accounts and set up real-time alerts to notify you when fund transfers are initiated.  Your bank will likely monitor for this and contact you for suspicious transactions.

2. Use Multi-Factor Authentication (MFA):
While AiTM attacks can sometimes bypass MFA, it’s still a critical layer of defense. MFA can make it more difficult for attackers to gain full control over your accounts, especially if they don’t capture the session token immediately.  Enable MFA requirements for all funds transfers over a single transaction threshold (such as $500).

3. Secure Your Browser
Keep Browsers Updated: Always use the latest version of your web browser, as updates often include fixes for known security vulnerabilities.
Remove Untrusted Extensions: Browser extensions can be exploited for BiTM attacks. Only use trusted and necessary browser add-ons.
Enable HTTPS Everywhere: Use browser extensions that enforce HTTPS on all sites you visit, ensuring your communications are encrypted.

4. Phishing Awareness
Many AiTM and BiTM attacks start with phishing. Be cautious of emails or links that ask you to log into your accounts. Always double-check URLs and avoid clicking on suspicious links.

5. Use a VPN on Public Networks
When using public Wi-Fi, a Virtual Private Network (VPN) can add an extra layer of encryption to your internet traffic, making it harder for attackers to intercept your session tokens.

6. Log Out Regularly
Manually log out of important websites, especially on shared or public devices. This prevents attackers from hijacking a still-active session.

7. Clear Cookies and Cache
Cookies store session tokens, so regularly clearing them can reduce your risk. Be sure to clear your browser’s cache and cookies periodically.

8. Consider Freezing Your Credit
If you suspect your personal information has been compromised through session hijacking or any other means, consider freezing your credit (instructions). This prevents attackers from opening new accounts in your name and protects you from identity theft.

## Conclusion:

Session Hijacking is a growing cyber threat, combining older hijacking techniques with sophisticated new attacks like AiTM, BiTM, and infostealers. These advanced tactics allow cybercriminals to bypass traditional defenses (passwords and MFA), putting both everyone at risk.
