---
title: "Major Supply Chain Attack on NPM Project NX"
description: >-
    A detailed technical analysis of a major supply chain attack on the Nx npm package, exposing how malicious code harvested sensitive credentials via Github.
author: anorak
date: 2025-08-30 04:30:00 +0530
categories: [NEWS, ISSUE]
tags: [MITRE ATT&CK, Threat Intelligence, Cybersecurity, Supply Chain Attack, Malware, Incident Response, GitHub, Vulnerability]
pin: True
---

## Major Supply Chain Attack on Nx: Technical Analysis

A significant supply chain attack has been identified in the NPM project Nx, a widely used build system and monorepo manager with millions of weekly downloads. This incident represents one of the most severe npm breaches in recent times, with far-reaching implications for the JavaScript and Node.js ecosystem.
 ![Nx Repository](/assets/img/202508/ss4.png){: width="800" height="600" .w-50 .center}

### What Happened

- **Affected Versions:** The malicious code was introduced in Nx versions 20.9.0, 20.10.0, 20.11.0, 20.12.0, 21.5.0, 21.6.0, 21.7.0, and 21.8.0.
- **Malicious Component:** A post-install script named `telemetry.js` was embedded in these versions. This script executed automatically upon package installation, regardless of user intent or awareness.

### In-Depth Analysis: What the Malware Does

The malicious `telemetry.js` script performed several sophisticated actions:

- **System-wide Secret Harvesting:**
  - The script recursively scanned the user's file system for sensitive files and credentials, including:
    - SSH private keys (commonly found in `~/.ssh/`)
    - Environment variable files (e.g., `.env`)
    - GitHub personal access tokens
    - Cryptocurrency wallet files
    - Configuration files for various development tools
  - By targeting these files, the attacker could obtain credentials that grant access to critical infrastructure, source code, and financial assets.

- **Automated Exfiltration to GitHub:**
  - The malware programmatically created a new repository named `s1ngularity-repository` in the victim's GitHub account using any discovered tokens.
  - Stolen data was encoded in base64 and uploaded as files to this repository.
  - This method leveraged GitHub's infrastructure for data exfiltration, making detection and takedown more challenging.
  ![Nx Supply Chain Attack](/assets/img/202508/nx-supply-chain-attack.png){: width="800" height="600" .w-50 .center}

- **Persistence and System Tampering:**
  - The script appended a command (`sudo shutdown -h 0`) to the user's `.bashrc` or `.zshrc` files.
  - This modification caused the system to shut down immediately upon the next terminal session, disrupting incident response and analysis.

### Criticality of the Attack

- **Exposure of Highly Sensitive Files:**
  - SSH keys and environment files often contain credentials for production servers, cloud providers, and CI/CD pipelines.
  - GitHub tokens can be used to access private repositories, modify code, and trigger further supply chain attacks.
  - Cryptocurrency wallets, if compromised, can result in direct financial loss.

- **Public Exfiltration Vector:**
  - By uploading stolen data to a public GitHub repository, the attacker ensured that secrets were easily accessible to anyone, not just the original threat actor.
  - This increases the risk of widespread abuse, as secrets can be harvested by automated bots or malicious actors monitoring public repositories.

- **Potential for Cascading Supply Chain Attacks:**
  - With access to npm and GitHub tokens, attackers could publish malicious packages, modify existing codebases, or escalate privileges within organizations.
  - The compromise of a widely used package like Nx amplifies the risk, as downstream projects and users may unknowingly propagate the attack.

### Immediate Remediation Steps

- **Check for Affected Versions:**
  - Run `npm ls nx` to determine if your project is using a compromised version.
- **If Affected:**
  - Uninstall the malicious package: `npm uninstall nx`
  - Reinstall a clean version: `npm install nx@latest`
  - Clear the npm cache: `npm cache clean --force`
- **Rotate All Secrets:**
  - Immediately revoke and regenerate all GitHub, npm, API tokens, and environment variables that may have been exposed.
  - Review access logs for suspicious activity.

### Conclusion

This attack highlights the critical importance of monitoring dependencies and understanding the potential impact of compromised credentials. The files and repositories targeted by this malware are foundational to the security and integrity of development and production environments. Organizations must act swiftly to mitigate the damage and prevent further exploitation.


