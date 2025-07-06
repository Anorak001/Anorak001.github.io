---  
title: Finding a Missing Subresource Integrity (SRI) Vulnerability  
description: >-
    How I found missing SRI using just curl and how it led to my first valid bug.
author: anorak
date: 2025-07-05 04:30:00 +0530
categories: [NEWS, BUG]
tags: [Web Security, Subresource Integrity, Bug Bounty, Reconnaissance, Cybersecurity]
pin: True
---   
 
 

##   Introduction

During a recent reconnaissance session on a web application, I discovered a **Missing Subresource Integrity (SRI)** vulnerability using only the `curl` command and browser inspection tools. While this issue is often considered low-severity or informational, it represents a **misconfiguration** that can have serious consequences if combined with other attack vectors, such as CDN compromise or Man-in-the-Middle (MITM) attacks.

![Valid_Bounty_Image](/assets/img/202507/img1.jpg){: .center}

This post details the technical process I used to identify the issue, explains the risks, and offers recommendations for remediation.

 

##   What is Subresource Integrity (SRI)?

**Subresource Integrity** is a browser security feature that ensures externally loaded resources (like scripts and stylesheets) have not been tampered with. It works by including a cryptographic hash in the `integrity` attribute of `<script>` or `<link>` tags. The browser verifies the fetched resource matches the expected hash before executing or applying it.

A secure resource inclusion looks like this:

```html
<script src="https://cdn.example.com/script.js" integrity="sha384-abc123..." crossorigin="anonymous"></script>
```

Without SRI, if an attacker compromises the external resource or intercepts network traffic, they could inject malicious code into your application—**and the browser would execute it without warning**.

 

##   Recon Setup

**Target:** A public-facing web application (company and asset details redacted for confidentiality).

**Goal:** Identify externally loaded JavaScript and CSS resources that lack SRI protection.

 

##   Using `curl` to Analyze Resource Inclusion

First, I fetched the HTML source of the homepage:

```bash
curl -s https://target-website.com/ > target.html
```

To extract all external `<script>` tags missing the `integrity` attribute:

```bash
grep -Eo '<script[^>]+src="[^"]+"' target.html | grep -v 'integrity='
```

For external stylesheets:

```bash
grep -Eo '<link[^>]+href="[^"]+"' target.html | grep -i 'stylesheet' | grep -v 'integrity='
```

This revealed a list of externally hosted assets—such as scripts from third-party CDNs and font providers—**none of which included an `integrity` attribute**.

 

##   Vulnerability Summary

###   Affected Resources

Examples of resources missing SRI (URLs and asset names redacted):

```text
https://cdn.example.com/assets/template_main.min.js
https://cdn.example.com/assets/template_video-modal.min.js
https://fonts.example.com/css?family=SomeFont
https://analytics.example.com/tags.js
```

These resources are included via `<script>` or `<link>` tags **without any integrity verification**.

###   Risk Analysis

If any of these external endpoints or CDNs are compromised, attackers could inject malicious JavaScript or CSS. This could lead to:

- Session hijacking
- DOM manipulation
- Data exfiltration
- Drive-by downloads

All **without any browser warnings**, since the integrity of the resource is not checked.

 

##   Responsible Disclosure

I reported this as a **P5-level misconfiguration** under the category:

```
VRT: Server Security Misconfiguration > Missing Subresource Integrity
```

While often marked as informational, this issue can be chained with other attacks (like CDN compromise or MITM) to escalate risk.

![Valid_Bounty_Image](/assets/img/202507/img2.jpg){: .center} 

##   Technical Deep Dive

### Why is SRI Important?

- **Supply Chain Security:** SRI helps protect against supply chain attacks by ensuring third-party resources haven’t been altered.
- **Defense in Depth:** Even if a CDN or external provider is compromised, SRI prevents malicious code from being executed.
- **Browser Enforcement:** Modern browsers support SRI and will block resources that fail integrity checks.

### Limitations

- SRI only works for resources loaded over HTTPS.
- Dynamic resources (with changing content) are harder to protect with SRI.
- Not all browsers enforce SRI for all resource types.

### Steps to Reproduce

1. Visit the target website.
2. Open Developer Tools and inspect the `<script>` and `<link>` tags.
3. Observe that many external resources lack the `integrity` attribute.

 

##   Recommendations

- **Add SRI hashes** to all externally loaded scripts and stylesheets.
- Use build tools or plugins to automate SRI hash generation and injection.
- Regularly audit third-party dependencies and monitor for supply chain risks.
- Consider using tools like [Nuclei](https://github.com/projectdiscovery/nuclei) to automate detection of missing SRI attributes.

 


##   Final Thoughts

Even seemingly minor issues like missing SRI can indicate gaps in a development team's security practices. Addressing these best practices helps harden your application against modern supply chain threats.

*Stay vigilant, automate where possible, and always validate findings manually for a deeper understanding of your application's security posture.*

