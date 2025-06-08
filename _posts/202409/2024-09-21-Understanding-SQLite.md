---
title: The SQLite Database
description: >-
 The Ubiquitous Database that Few Understand
author: anorak
date: 2024-09-21 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Database,  Security, Vulnerabilities, Cybersecurity, Forensics]
pin: false
---
Welcome back!

While most IT and cybersecurity professionals are familiar with the big database management systems such as Oracle, DB2, MS SQL Server, MySQL, and postgresql, few understand that most widely used database in the world, sqlite! That;s likely because it quietly being used in everything from your phone to your browser to IoT devices to store and organize data for your application or device. This database can be a treasure drove for the hacker or forensic investigator looking for information on the target.

SQLite is a powerful, compact, and versatile relational database engine that has become ubiquitous in modern computing. Unlike traditional database management systems, SQLite operates as an embedded database, requiring no separate server process or configuration. This unique design has led to its widespread adoption across a diverse range of devices and applications, from smartphones and web browsers to desktop software and Internet of Things (IoT) devices. 

SQLite's functionality has made it an indispensable tool for developers and a crucial component in many of the technologies we use daily. But developers don't always pay attention to security. That's why in this article, let's take a closer look at how SQLite works, how and where it can be used, and what vulnerabilities it hides.

## Background:

The development of SQLite began in 2000 when D. Richard Hipp was working on a database solution for the U.S. Navy's guided missile destroyer program. The project required a reliable database that could operate without administrative oversight. This need led Hipp to create SQLite, a self-contained, serverless, zero-configuration, and transactional SQL database engine.

Key milestones in SQLite's history include:

  -   2000: Initial development begins

  -   2006: Adopted as part of Android OS

  -   2007: Included in Apple's iOS

  -   2013: SQLite reaches 1 trillion databases worldwide



SQLite's widespread adoption in mobile operating systems and diverse software applications has established its status as one of the most widely deployed database engines in the world.





## How It Works:

SQLite operates as an embedded database engine, meaning it runs as part of the application rather than as a separate process.


While there are numerous differences between SQL and SQLite, here are some fundamental distinctions:

 1.   SQL is a standardized query language used by various relational database management systems such as MySQL, PostgreSQL, and SQLite. In contrast, SQLite is a specific implementation that uses SQL.

2.    SQL is a query language, whereas SQLite is a self-contained database engine.

  3.  SQL is used to query and manage relational database systems, while SQLite is a lightweight, serverless Relational Database Management System (RDBMS).

 4.   SQL provides a standard for creating and manipulating relational schemas across different database systems. SQLite, on the other hand, is a file-based database that implements most SQL standards.

## USAGE:

- Internet of Things (IoT) devices: SQLite stores sensor data and device configurations in IoT devices, offering reliable local storage with a small footprint.

- Operating systems: Many operating systems use SQLite for internal databases, managing system data like search indexes and package information.

- Web browsers: Major web browsers employ SQLite to store user data such as bookmarks, browsing history, and cookies.

- Desktop software: SQLite serves as a file format and local database for many desktop applications, storing user data and application settings.

## SQLite Security Vulnerabilities:

One of the most intriguing vulnerabilities demonstrated was the potential for SQLite databases to be exploited by threat actors as an attack vector to execute malicious code in other applications.

“We discovered that simply querying a malicious SQLite database can lead to Remote Code Execution. We leveraged undocumented SQLite3 behavior and memory corruption vulnerabilities to exploit the assumption that querying a database is safe,” explained Omer Gull, a security researcher at Check Point. “We created a rogue SQLite database that exploits the software used to open it.”

The study conducted by the researchers demonstrated how to exploit memory corruption issues in SQLite using only the SQL language. The experts developed techniques such as Query Hijacking and Query-Oriented Programming to trigger these issues within the SQLite engine. Gull showcased a couple of real-world scenarios: in one, he hacked the command-and-control (C2) server of a password stealer, and in another, he demonstrated how to achieve persistent iOS access with elevated privileges.

![img](/assets/img/202409/sqllite.webp){: width="600" height="600" .left}

The attack technique leverages vulnerabilities in the process third-party apps use to read data from SQLite databases. The researchers were able to store malicious code within an SQLite database used by third-party applications. Once these applications accessed the database, the malicious code was executed.

At DEF CON, the researchers demonstrated how SQLite databases could be used to execute malware when accessed by iMessage. An attacker could replace or modify the “AddressBook.sqlitedb” file, injecting malware into an iPhone’s address book. When iMessage queries this SQLite file, it would trigger the execution of the malicious code, allowing the malware to gain persistence on the device.

“Furthermore, the contacts database is shared among many processes, including Contacts, FaceTime, Springboard, WhatsApp, Telegram, and XPCProxy. Some of these processes are more privileged than others,” reads the analysis published by Check Point. “Once we proved that we could execute code within the context of the querying process, this technique also allowed us to expand and elevate our privileges.”

Experts reported their findings to Apple, and the issues were assigned the following CVEs:

   -  CVE-2019-8600

  - CVE-2019-8598

   -  CVE-2019-8602

  - CVE-2019-8577

![img](/assets/img/202409/sqllite2.webp){: width="500" height="500" .center}

Security researchers believe this is just the beginning of exploring SQLite's exploitation potential. The techniques developed are not exclusive to SQLite and could potentially be adapted to other SQL engines, opening up new avenues for both attack and defense in the cybersecurity landscape.

## Summary

The cat-and-mouse game between attackers and defenders continues to evolve, with SQLite as just one of many battlegrounds. As technology continues to evolve, SQLite's role in the digital landscape is likely to grow, making it an essential subject of study for anyone involved in cybersecurity.


