---
title: Malicious Repos on Github
description: >-
 An article on the repo confusiong campaign attack
author: anorak
date: 2024-08-09 04:55:00 +0530
categories: [NEWS,ISSUE]
tags: [Cybersecurity,malware]
pin: false
---

## VAULT FOR MALWARE STORAGE:
Its no wonder that few lazy devs store their api keys openly on their github repo.But, recently, it was found that there are more than 100,000+ repositories infected with malware.

## The Repository Confusion Campaign:

- The attack impacts more than 100,000 GitHub repositories (and presumably millions) when unsuspecting developers use repositories that resemble known and trusted ones but are, in fact, infected with malicious code. 

### How do these Attacks Happen?

-  Similar to dependency confusion attacks, malicious actors get their target to download their malicious version instead of the real one.
-  But dependency confusion attacks take advantage of how package managers work, while repo confusion attacks simply rely on humans to mistakenly pick the malicious version over the real one, sometimes employing social engineering techniques as well.
-  In this case, in order to maximize the chances of infection, the malicious actor is flooding GitHub with malicious repos, following these steps:

    1. Cloning existing repos (for example: TwitterFollowBot, WhatsappBOT, discord-boost-tool, Twitch-Follow-Bot, and hundreds more).
    2. Infecting them with malware loaders.
    3. Uploading them back to GitHub with identical names. 
    4. Automatically forking each thousands of times. 
    5. Covertly promoting them across the web via forums, discord, etc.
 
### What happens when these repositories are in use?

- Once unsuspecting developers use any of the malicious repos, the hidden payload unpacks seven layers of obfuscation, which also involves pulling malicious Python code and later a binary executable.
- The malicious code (largely a modified version of BlackCap-Grabber) would then collect login credentials from different apps, browser passwords and cookies, and other confidential data.
- It then sends it back to the malicious actors’ C&C (command-and-control) server and performs a long series of additional malicious activities.

## Indicators of Compromise(How to know if you are infected):

- Search for the following Python patterns and investigate any matches:
  
           - exec(Fernet
           - exec(requests 
           - exec(__import
           - exec(bytes
           - exec(“””\nimport
           - exec(compile
           - __import__(“builtins”).exec(
 
  
 - Check for the local presence of any repositories related to automations of actions on social platforms, bots, and gaming, and remove them. If you must, then reinstall – but this time carefully verify the source, and either avoid it or run it in a sandbox.
 - If you believe there’s a chance a repository of this type was cloned, respond as if the following cookies, credentials and keys were stolen:
        - From browsers: any financial services, any email services, any crypto services, Amazon, eBay, AliExpress, Facebook, Instagram, Twitter, Youtube, Discord, TikTok, Telegram, Twitch, Steam, Yahoo, ExpressVPN, Spotify, and any streaming services.
        - From apps: Exodus, Atomic Wallet, Guarda, Coinomi, Ethereum.
- If you would like to verify files checksums, the length of the list is impractical but some of the common ones can be found in [this VirusTotal graph](https://www.virustotal.com/graph/embed/gcaa313af04de4e9dba8fd990fa41444e370ecb32e35444e3a8109dfe8b647456?theme=dark).

## Aftermath:

- GitHub was notified, and most of the malicious repos were deleted, but the campaign continues, and attacks that attempt to inject malicious code into the supply chain are becoming increasingly prevalent.
- here are countless solutions for catching malware at the system and network levels, but the supply chain remains a massive and lucrative attack surface for malicious actors. If you encounter any malicious repo, part of this campaign or not, we encourage you to [report](https://docs.github.com/en/communities/maintaining-your-safety-on-github/reporting-abuse-or-spam) it.
  

