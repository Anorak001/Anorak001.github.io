---
title: "  The Google Dorking Cheat Sheet  "
description: >-
      A Comprehensive Guide to Advanced Google Search Operators for OSINT  
author: anorak
date: 2026-01-11 03:10:00 +0530
categories: [ GUIDE, CYBERSECURITY]
tags: [Cybersecurity,  CEH, OSINT, Google Dorking ]
pin: True
--- 
 
 

## Introduction

Google Dorking (or Google Hacking) is the practice of using advanced search operators to filter the massive index of the web to find security holes, configuration leaks, and sensitive data.

For a Red Teamer, this is **passive reconnaissance**. You are querying Google's servers, not the target's. This means zero logs are generated on the victim's infrastructure until you actually click a result.

Below is a categorized cheat sheet of the most effective operators and combinations used in modern assessments.

---

## 1. The Core Operators

These are the building blocks. You rarely use them alone; you combine them to filter out the noise.

| Operator | Description | Example |
| --- | --- | --- |
| **`site:`** | Limits results to a specific domain or TLD. | `site:tesla.com` or `site:.gov` |
| **`filetype:`** | Searches for specific file extensions. (Alias: `ext:`) | `filetype:pdf` or `ext:docx` |
| **`inurl:`** | Looks for a string specifically inside the URL. | `inurl:admin` or `inurl:login` |
| **`intitle:`** | Looks for a string in the browser tab title. | `intitle:"index of"` |
| **`intext:`** | Searches for a string in the body text of the page. | `intext:"password"` |
| **`cache:`** | Shows Google's cached version of a site (great for deleted content). | `cache:target.com` |
| **`-`** | The "Exclude" operator. Removes results containing a word. | `site:linkedin.com -inurl:pub` |
| **`*`** | The "Wildcard". Matches any word or phrase. | `site:target.com password *` |

---

## 2. "The Recipes" (Common Attack Vectors)

Copy-paste these combinations to hunt for specific vulnerabilities.

###   Directory Listing (The "Index Of")

Finds web servers that are misconfigured to list all their files openly.

* **Standard:** `intitle:"index of" "parent directory"`
* **Specific Files:** `intitle:"index of" "dcim"` (Often finds unsecure camera backups)
* **Server Specific:** `intitle:"index of" "server at"`

###   Configuration & Secret Hunting

Finds files that developers accidentally exposed.

* **SSH Keys:** `intitle:"index of" id_rsa` or `intitle:"index of" id_dsa`
* **Environment Variables:** `ext:env intext:APP_KEY` or `ext:env intext:DB_PASSWORD`
* **Log Files:** `ext:log "software caused connection abort"` (Finds error logs that may leak paths)
* **SQL Dumps:** `filetype:sql intext:password | intext:"dumped by"`

###   Admin Portals

Finds the login door so you can try to pick the lock.

* **General:** `site:target.com inurl:admin`
* **Login Pages:** `site:target.com (inurl:login | inurl:signin | intitle:login)`
* **Specific Tech:** `site:target.com inurl:wp-admin` (WordPress)

###   Document Harvesting

Great for finding usernames, email formats, and metadata.

* **Spreadsheets:** `site:target.com filetype:xls OR filetype:xlsx "email"`
* **PDFs:** `site:target.com filetype:pdf "confidential"`
* **Text Files:** `site:target.com filetype:txt "password"`

---

## 3. Vulnerability Mining

Red Teamers use these to find potential entry points for SQL Injection (SQLi) or Local File Inclusion (LFI).

* **Potential SQLi:** `site:target.com inurl:index.php?id=`
* **Potential LFI:** `site:target.com inurl:page=` or `inurl:file=`
* **Open Redirects:** `site:target.com inurl:redir=` or `inurl:url=`

---

## 4. The "GHDB" Categories

The **Google Hacking Database (GHDB)** maintains thousands of these dorks. When you are looking for something specific, check the Exploit-DB categories:

1. **Footholds:** Login portals, VPN endpoints.
2. **Files Containing Usernames:** Helpful for brute-force list generation.
3. **Sensitive Directories:** `/proc/self/cwd` or `.git` folders.
4. **Web Server Detection:** Identifying version numbers (e.g., "Apache/2.4.49").
5. **Vulnerable Files:** Known vulnerable scripts (e.g., older versions of jQuery or PHP components).

---

##  Defense: Robots.txt & Meta Tags

If you are a defender, how do you stop this?

1. **`robots.txt`:**
Place this file in your root directory.
```text
User-agent: *
Disallow: /admin/
Disallow: /private/
Disallow: *.sql

```


*Note: This is a polite request. Good bots (Google) respect it. Bad bots ignore it.*
2. **NoIndex Meta Tag:**
Place this in the HTML header of sensitive pages.
```html
<meta name="robots" content="noindex, nofollow">

```


This removes the specific page from Google's index entirely.

---

##  Important Legal Warning

**Searching is legal. Exploiting is not.**

* It is legal to type `filetype:sql intext:password` into Google.
* It is **illegal** to download that database, log into the server using the credentials you found, or use the data for blackmail.

As a CEH or OSCP candidate, use these techniques only on infrastructure you own or have explicit permission to audit.

**Happy Hunting.**