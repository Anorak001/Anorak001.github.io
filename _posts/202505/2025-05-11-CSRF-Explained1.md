---
title: Testing for CSRF
description: >-
 A Tutorial Blog on how to check for CSRF Vulnerability
author: anorak
date: 2025-05-11 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity, Vulnerability, Web Security, Penetration Testing, Application Security]
pin: False
---


# Cross-Site Request Forgery (CSRF) Attack Overview

## Summary

Cross-Site Request Forgery (CSRF) is an attack that forces an end user to execute unintended actions on a web application in which they are currently authenticated. With a little social engineering help (like sending a link via email or chat), an attacker may force the users of a web application to execute actions of the attacker’s choosing. A successful CSRF exploit can compromise end user data and operation when it targets a normal user. If the targeted end user is the administrator account, a CSRF attack can compromise the entire web application.
![img](assets/img/202505/Session_riding.GIF){: width="800" height="800"  .center}
## Key Points of CSRF Vulnerability

- Web browser behavior regarding the handling of session-related information such as cookies and HTTP authentication information.
- An attacker’s knowledge of valid web application URLs, requests, or functionality.
- Application session management relying only on information known by the browser.
- Existence of HTML tags whose presence cause immediate access to an HTTP[S] resource; for example, the image tag `<img>`.

Points 1, 2, and 3 are essential for the vulnerability to be present, while point 4 facilitates the actual exploitation, but is not strictly required.

## How CSRF Works

- Browsers automatically send information used to identify a user session (e.g., cookies).
- Attackers can identify application URLs and parameters by code analysis or by accessing the application.
- Information such as cookies or HTTP-based authentication is stored by the browser and sent with each request.
- GET and POST requests can both be exploited by CSRF.

## Example: Session Riding

The GET request could be sent by the user in several different ways:

- Using the web application
- Typing the URL directly in the browser
- Following an external link that points to the URL

These invocations are indistinguishable by the application. For example, an attacker can embed a malicious image tag in an email:

```html
<img src="https://www.company.example/action" width="0" height="0">
```

When the browser displays this page, it will try to display the specified zero-dimension (thus, invisible) image, resulting in a request being automatically sent to the web application.

## Example: Firewall Management Console

Suppose a firewall web management console allows an authenticated user to delete a rule by its ID or all rules by specifying `*`. The following GET request would delete all firewall rules:

```
https://www.company.example/fwmgt/delete?rule=*
```

## Test Objectives

- Determine whether it is possible to initiate requests on a user’s behalf that are not initiated by the user.

## How to Test for CSRF

1. Audit the application’s session management. If it relies only on client-side values (cookies, HTTP authentication), it is vulnerable.
2. Resources accessible via HTTP GET requests are easily vulnerable.
3. POST requests can also be automated and are vulnerable.
4. Use the following sample for POST-based CSRF testing:

```html
<html>
<body onload='document.CSRF.submit()'>
<form action='https://targetWebsite/Authenticate.jsp' method='POST' name='CSRF'>
    <input type='hidden' name='name' value='Hacked'>
    <input type='hidden' name='password' value='Hacked'>
</form>
</body>
</html>
```

## CSRF with JSON Payloads

In case of web applications in which developers are utilizing JSON for browser to server communication, a problem may arise because there are no query parameters with the JSON format, which are required for self-submitting forms. To bypass this, we can use a self-submitting form with JSON payloads, including hidden input, to exploit CSRF. We’ll have to change the encoding type (`enctype`) to `text/plain` to ensure the payload is delivered as-is. The exploit code will look like the following:

```html
<html>
 <body>
  <script>history.pushState('', '', '/')</script>
   <form action='https://victimsite.com' method='POST' enctype='text/plain'>
     <input type='hidden' name='{"name":"hacked","password":"hacked","padding":"'value='something"}' />
     <input type='submit' value='Submit request' />
   </form>
 </body>
</html>
```

The POST request will be as follows:

```http
POST / HTTP/1.1
Host: victimsite.com
Content-Type: text/plain

{"name":"hacked","password":"hacked","padding":"=something"}
```

When this data is sent as a POST request, the server will accept the `name` and `password` fields and ignore the one with the name `padding` as it does not need it.

