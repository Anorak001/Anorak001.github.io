---
title: Understanding SQLI 01
description: >-
  A detailed blog on Basics of SQLi
author: anorak
date: 2024-11-17 00:10:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity]
pin: false
---

### what is SQLI?


SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure. It can also enable them to perform denial-of-service attacks.

![img](/assets/img/202411/sqli.svg){: width="600" height="600" .center}

### What is the impact of a successful SQL injection attack?
 A successful SQL injection attack can result in unauthorized access to sensitive data, such as:

  -  Passwords.
  -  Credit card details.
  -  Personal user information.

SQL injection attacks have been used in many high-profile data breaches over the years. These have caused reputational damage and regulatory fines. In some cases, an attacker can obtain a persistent backdoor into an organization's systems, leading to a long-term compromise that can go unnoticed for an extended period. 

### How to detect SQL injection vulnerabilities?
 You can detect SQL injection manually using a systematic set of tests against every entry point in the application. To do this, you would typically submit:

   The single quote character ' and look for errors or other anomalies.
   Some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and look for systematic differences in the application responses.
   Boolean conditions such as OR 1=1 and OR 1=2, and look for differences in the application's responses.
   Payloads designed to trigger time delays when executed within a SQL query, and look for differences in the time taken to respond.
   OAST payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitor any resulting interactions.

Alternatively, you can find the majority of SQL injection vulnerabilities quickly and reliably using Burp Scanner. 

### SQL injection in different parts of the query:

Most SQL injection vulnerabilities occur within the WHERE clause of a SELECT query. Most experienced testers are familiar with this type of SQL injection.

However, SQL injection vulnerabilities can occur at any location within the query, and within different query types. Some other common locations where SQL injection arises are:

-   In UPDATE statements, within the updated values or the WHERE clause.
 - In INSERT statements, within the inserted values.
 - In SELECT statements, within the table or column name.
 - In SELECT statements, within the ORDER BY clause.
 - 
### SQL injection examples

There are lots of SQL injection vulnerabilities, attacks, and techniques, that occur in different situations. Some common SQL injection examples include:

1.    Retrieving hidden data, where you can modify a SQL query to return additional results.
2.    Subverting application logic, where you can change a query to interfere with the application's logic.
3.    UNION attacks, where you can retrieve data from different database tables.
4.    Blind SQL injection, where the results of a query you control are not returned in the application's responses.

### How to prevent SQL injection

You can prevent most instances of SQL injection using parameterized queries instead of string concatenation within the query. These parameterized queries are also know as "prepared statements".

The following code is vulnerable to SQL injection because the user input is concatenated directly into the query:
String query = "SELECT * FROM products WHERE category = '"+ input + "'";
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(query);

You can rewrite this code in a way that prevents the user input from interfering with the query structure:
PreparedStatement statement = connection.prepareStatement("SELECT * FROM products WHERE category = ?");
statement.setString(1, input);
ResultSet resultSet = statement.executeQuery();

You can use parameterized queries for any situation where untrusted input appears as data within the query, including the WHERE clause and values in an INSERT or UPDATE statement. They can't be used to handle untrusted input in other parts of the query, such as table or column names, or the ORDER BY clause. Application functionality that places untrusted data into these parts of the query needs to take a different approach, such as:

-    Whitelisting permitted input values.
 -   Using different logic to deliver the required behavior.

For a parameterized query to be effective in preventing SQL injection, the string that is used in the query must always be a hard-coded constant. It must never contain any variable data from any origin. Do not be tempted to decide case-by-case whether an item of data is trusted, and continue using string concatenation within the query for cases that are considered safe. It's easy to make mistakes about the possible origin of data, or for changes in other code to taint trusted data. 

