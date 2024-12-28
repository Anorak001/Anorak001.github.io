---
title: BLIND!
description: >-
 Detailed blog on Blind SQL injection
author: anorak
date: 2024-12-01 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity]
pin: false
---
## What is blind SQL injection? 

Blind SQL injection occurs when an application is vulnerable to SQL injection, but its HTTP responses do not contain the results of the relevant SQL query or the details of any database errors.

Many techniques such as UNION attacks are not effective with blind SQL injection vulnerabilities. This is because they rely on being able to see the results of the injected query within the application's responses. It is still possible to exploit blind SQL injection to access unauthorized data, but different techniques must be used. 

## Exploiting blind SQL injection by triggering conditional responses:

 Consider an application that uses tracking cookies to gather analytics about usage. Requests to the application include a cookie header like this:
`Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4`

When a request containing a TrackingId cookie is processed, the application uses a SQL query to determine whether this is a known user:
`SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'`

This query is vulnerable to SQL injection, but the results from the query are not returned to the user. However, the application does behave differently depending on whether the query returns any data. If you submit a recognized TrackingId, the query returns data and you receive a "Welcome back" message in the response.

This behavior is enough to be able to exploit the blind SQL injection vulnerability. You can retrieve information by triggering different responses conditionally, depending on an injected condition.

To understand how this exploit works, suppose that two requests are sent containing the following TrackingId cookie values in turn:
```
…xyz' AND '1'='1
…xyz' AND '1'='2
```

 - The first of these values causes the query to return results, because the injected AND '1'='1 condition is true. As a result, the "Welcome back" message is displayed.
 - The second value causes the query to not return any results, because the injected condition is false. The "Welcome back" message is not displayed.

This allows us to determine the answer to any single injected condition, and extract data one piece at a time.

For example, suppose there is a table called Users with the columns Username and Password, and a user called Administrator. You can determine the password for this user by sending a series of inputs to test the password one character at a time.

To do this, start with the following input:
```xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm```

This returns the "Welcome back" message, indicating that the injected condition is true, and so the first character of the password is greater than m.

Next, we send the following input:
`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 't`

This does not return the "Welcome back" message, indicating that the injected condition is false, and so the first character of the password is not greater than t.

Eventually, we send the following input, which returns the "Welcome back" message, thereby confirming that the first character of the password is s:
`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) = 's`

We can continue this process to systematically determine the full password for the Administrator user. 

## Error-based SQL injection

Error-based SQL injection refers to cases where you're able to use error messages to either extract or infer sensitive data from the database, even in blind contexts. The possibilities depend on the configuration of the database and the types of errors you're able to trigger:

-   You may be able to induce the application to return a specific error response based on the result of a boolean expression. You can exploit this in the same way as the conditional responses we looked at in the previous section. For more information, see Exploiting blind SQL injection by triggering conditional errors.
-   You may be able to trigger error messages that output the data returned by the query. This effectively turns otherwise blind SQL injection vulnerabilities into visible ones. For more information, see Extracting sensitive data via verbose SQL error messages.

### Exploiting blind SQL injection by triggering conditional errors:

Some applications carry out SQL queries but their behavior doesn't change, regardless of whether the query returns any data. The technique in the previous section won't work, because injecting different boolean conditions makes no difference to the application's responses.

It's often possible to induce the application to return a different response depending on whether a SQL error occurs. You can modify the query so that it causes a database error only if the condition is true. Very often, an unhandled error thrown by the database causes some difference in the application's response, such as an error message. This enables you to infer the truth of the injected condition.

To see how this works, suppose that two requests are sent containing the following TrackingId cookie values in turn:
`xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a`
`xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a`

These inputs use the CASE keyword to test a condition and return a different expression depending on whether the expression is true:

 -   With the first input, the CASE expression evaluates to 'a', which does not cause any error.
 -   With the second input, it evaluates to 1/0, which causes a divide-by-zero error.

If the error causes a difference in the application's HTTP response, you can use this to determine whether the injected condition is true.

Using this technique, you can retrieve data by testing one character at a time:
`xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a`

### Extracting sensitive data via verbose SQL error messages

Misconfiguration of the database sometimes results in verbose error messages. These can provide information that may be useful to an attacker. For example, consider the following error message, which occurs after injecting a single quote into an id parameter:
`Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = '''. Expected char`

This shows the full query that the application constructed using our input. We can see that in this case, we're injecting into a single-quoted string inside a WHERE statement. This makes it easier to construct a valid query containing a malicious payload. Commenting out the rest of the query would prevent the superfluous single-quote from breaking the syntax.

Occasionally, you may be able to induce the application to generate an error message that contains some of the data that is returned by the query. This effectively turns an otherwise blind SQL injection vulnerability into a visible one.

You can use the CAST() function to achieve this. It enables you to convert one data type to another. For example, imagine a query containing the following statement:
CAST((SELECT example_column FROM example_table) AS int)

Often, the data that you're trying to read is a string. Attempting to convert this to an incompatible data type, such as an int, may cause an error similar to the following:
ERROR: invalid input syntax for type integer: "Example data"

This type of query may also be useful if a character limit prevents you from triggering conditional responses. 


## Exploiting Blind SQL:

### Exploiting blind SQL injection by triggering time delays

If the application catches database errors when the SQL query is executed and handles them gracefully, there won't be any difference in the application's response. This means the previous technique for inducing conditional errors will not work.

In this situation, it is often possible to exploit the blind SQL injection vulnerability by triggering time delays depending on whether an injected condition is true or false. As SQL queries are normally processed synchronously by the application, delaying the execution of a SQL query also delays the HTTP response. This allows you to determine the truth of the injected condition based on the time taken to receive the HTTP response.

The techniques for triggering a time delay are specific to the type of database being used. For example, on Microsoft SQL Server, you can use the following to test a condition and trigger a delay depending on whether the expression is true:
`'; IF (1=2) WAITFOR DELAY '0:0:10'--`
`'; IF (1=1) WAITFOR DELAY '0:0:10'--`

-    The first of these inputs does not trigger a delay, because the condition 1=2 is false.
-   The second input triggers a delay of 10 seconds, because the condition 1=1 is true.

Using this technique, we can retrieve data by testing one character at a time:
`'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--`


## How to prevent blind SQL injection attacks?

Although the techniques needed to find and exploit blind SQL injection vulnerabilities are different and more sophisticated than for regular SQL injection, the measures needed to prevent SQL injection are the same.

As with regular SQL injection, blind SQL injection attacks can be prevented through the careful use of parameterized queries, which ensure that user input cannot interfere with the structure of the intended SQL query. 


