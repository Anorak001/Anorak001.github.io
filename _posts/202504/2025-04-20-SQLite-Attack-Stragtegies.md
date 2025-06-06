--- 
title: Database Hacking
description: >-
  Blog on SQLite Essentials and Attack Strategies
author: anorak
date: 2025-04-20 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity, Database, Vulnerabilities,  Malware, Security]
pin: True
---  

Welcome back !

SQLite is one of the most commonly used database engines, embedded in countless applications, mobile devices, and web services. Its compact design makes it a popular choice for developers, but it’s also an attractive target for hackers. Whether you’re a penetration tester, cyberwarrior, or security researcher, mastering SQLite can open up new vectors for exploitation. In this guide, we’ll delve into SQLite’s basic structure and explore how hackers leverage it during attacks.

 

## How Notorious Hacker Groups Utilize SQLite Databases

Let’s look at some real-world examples of how sophisticated hacking groups have used SQLite databases in their operations:

### APT Groups and SQLite Misuse

Advanced Persistent Threat (APT) groups often exploit SQLite databases embedded in software or firmware. They gain access to these databases during attacks on web servers, compromised applications, or mobile devices. For example, state-sponsored groups like **APT28 (Fancy Bear)** and **Lazarus Group** have leveraged SQLite as a reconnaissance tool:

- They extract usernames, application secrets, and configurations to gather intelligence.
- SQLite databases on compromised servers often store logs, enabling attackers to analyze and pivot deeper into the target environment.

### Ransomware Operators

Ransomware groups, such as **REvil** or **Conti**, often seek SQLite files to extract credentials or escalate their attacks. Ransomware binaries are known to scan systems for `.db` or `.sqlite` files, stealing any unencrypted credentials, and even altering databases to corrupt application operations as part of the ransom demand strategy.

### Credential Stealers and Data Exfiltration

Groups like **Emotet** and **TrickBot** — known for deploying malware that specifically targets browser SQLite databases — frequently extract saved usernames and passwords from browser SQLite files (`Login Data`, `Cookies`, etc.).

> Understanding how SQLite databases are targeted in real-world attacks is important for acting proactively, not reactively — implementing countermeasures and strengthening security.

 

## Using SQLite on Linux

Before using SQLite, check if it’s installed:

```bash
kali> sqlite3 --version
```

To create or open a database:

```bash
kali> sqlite3 <db_name>
```

Inside the SQLite prompt:

### Create a table:

```sql
sqlite> CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT NOT NULL, age INTEGER);
```


![img](/assets/img/202504/m1.avif){: width="600" height="800"  .center}

This creates a `users` table with:

- `id`: An integer primary key
- `name`: A text field (NOT NULL)
- `age`: An optional integer

### Insert data:

```sql
sqlite> INSERT INTO users (name, age) VALUES ('Admin', 30);
```

### Retrieve data:

```sql
sqlite> SELECT * FROM users;
```

### View schema:

```sql
sqlite> PRAGMA table_info(users);
```



![img](/assets/img/202504/m2.avif){: width="600" height="800"  .center}

### List all tables:

```sql
sqlite> .tables
```

### Update a record:

```sql
sqlite> UPDATE users SET role='admin' WHERE username='guest';
```

### Modify security settings:

```sql
sqlite> UPDATE config SET value='false' WHERE key='authentication_required';
```

### Export entire database:

```sql
sqlite> .dump
```

 

## Attack Strategies

SQLite databases have been a target of numerous sophisticated attacks. Let’s explore some of the major ones:

### 1. Memory Corruption and Remote Code Execution (RCE)

The **Magellan vulnerabilities** (e.g., CVE-2019-5018, CVE-2019-13734) exploited flaws in SQLite’s implementation to allow remote code execution. These were triggered via specially crafted SQL queries that caused:

- Heap-based buffer overflows
- Out-of-bounds reads

A vulnerable function was `fts3_tokenizer` in the Full-Text Search (FTS) module. Attackers embedded malicious payloads in web pages, exploiting browsers like **Chromium** that use SQLite internally.

### 2. Database File Manipulation & Malicious Trigger Injection

Attackers can tamper directly with `.db` files to insert **triggers** — commands that auto-execute on certain actions (INSERT, UPDATE). In mobile apps, attackers modify local `.db` files to:

- Exfiltrate data
- Alter app behavior
- Inject malicious code

### 3. Arbitrary Code Execution via Query Parsing Bugs

Example: **CVE-2020-15358**  
By exploiting flaws in how SQLite parses nested `WITH` clause queries, attackers manipulated memory routines and achieved arbitrary code execution.

Dangerous when:
- Web applications allow user-controlled queries.
- Sanitization is weak or bypassed.

### 4. Exploiting Privileges & Weak Encryption in Mobile Apps

Many mobile apps use unencrypted SQLite databases to store sensitive data. For example:

- WhatsApp once stored chat data unencrypted in SQLite format.


![img](/assets/img/202504/m3.avif){: width="600" height="800"  .center}

- Attackers extract session data, tokens, or credentials.

### 5. Privilege Escalation via `load_extension` Abuse

SQLite’s `load_extension` function allows dynamic loading of external modules (`.so`, `.dll` files). If enabled:

- Attackers can load arbitrary shared libraries.
- Gain code execution at the same privilege level as the SQLite process.

Example: On an IoT device, `load_extension` was left enabled for debugging. Attackers exploited it to inject custom modules and take control.

 

## Summary

This article is a hacker's guide to SQLite, covering both basics and attack strategies. It introduces SQLite commands and structure, then explores how various threat actors exploit SQLite vulnerabilities. The content is crucial for cybersecurity professionals and developers, as it provides essential knowledge about a widely used database system while highlighting its potential security risks.
