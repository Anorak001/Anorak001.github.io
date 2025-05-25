---
title: Understanding XSS 01
description: >-
  A Beginner’s Guide To Cross Site Scripting
author: anorak
date: 2024-11-09 00:10:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity]
pin: false
---
## Introduction:

Cross-Site Scripting (XSS) attacks are a type of injection, in which malicious scripts are injected into otherwise benign and trusted websites. XSS attacks occur when an attacker uses a web application to send malicious code, generally in the form of a browser side script, to a different end user. Flaws that allow these attacks to succeed are quite widespread and occur anywhere a web application uses input from a user within the output it generates without validating or encoding it.

## Description:

Cross-Site Scripting (XSS) attacks occur when:

  1.  Data enters a Web application through an untrusted source, most frequently a web request.
  2.  The data is included in dynamic content that is sent to a web user without being validated for malicious content.

The malicious content sent to the web browser often takes the form of a segment of JavaScript, but may also include HTML, Flash, or any other type of code that the browser may execute.
The variety of attacks based on XSS is almost limitless, but they commonly include transmitting private data, like cookies or other session information, to the attacker, redirecting the victim to web content controlled by the attacker, or performing other malicious operations on the user’s machine under the guise of the vulnerable site.

## Reflected and Stored XSS Attacks:
XSS attacks can generally be categorized into two categories: reflected and stored. There is a third, much less well-known type of XSS attack called DOM Based XSS that will be discussed separately in the future.

### Reflected XSS Attacks:

Reflected attacks are those where the injected script is reflected off the web server, such as in an error message, search result, or any other response that includes some or all of the input sent to the server as part of the request. Reflected attacks are delivered to victims via another route, such as in an e-mail message, or on some other website. When a user is tricked into clicking on a malicious link, submitting a specially crafted form, or even just browsing to a malicious site, the injected code travels to the vulnerable web site, which reflects the attack back to the user’s browser. The browser then executes the code because it came from a “trusted” server. Reflected XSS is also sometimes referred to as Non-Persistent or Type-I XSS (the attack is carried out through a single request / response cycle).

### Stored XSS Attacks:

Stored attacks are those where the injected script is permanently stored on the target servers, such as in a database, in a message forum, visitor log, comment field, etc. The victim then retrieves the malicious script from the server when it requests the stored information. Stored XSS is also sometimes referred to as Persistent or Type-II XSS.

### Blind Cross-site Scripting:

Blind Cross-site Scripting is a form of persistent XSS. It generally occurs when the attacker’s payload saved on the server and reflected back to the victim from the backend application. For example in feedback forms, an attacker can submit the malicious payload using the form, and once the backend user/admin of the application will open the attacker’s submitted form via the backend application, the attacker’s payload will get executed. Blind Cross-site Scripting is hard to confirm in the real-world scenario but one of the best tools for this is XSS Hunter.

## How to protect yourself:

The primary defenses against XSS are described in the [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).

Also, it’s crucial that you turn off HTTP TRACE support on all web servers. An attacker can steal cookie data via Javascript even when document.cookie is disabled or not supported by the client. This attack is mounted when a user posts a malicious script to a forum so when another user clicks the link, an asynchronous HTTP Trace call is triggered which collects the user’s cookie information from the server, and then sends it over to another malicious server that collects the cookie information so the attackercan mount a session hijack attack. This is easily mitigated by removing support for HTTP TRACE on all web servers.

## Alternate XSS Syntax:
### XSS Using Script in Attributes Alternate XSS Syntax:


XSS attacks may be conducted without using ``` <script>...</script>``` tags. Other tags will do exactly the same thing, for example:``` <body onload=alert('test1')> ```or other attributes like: ```  onmouseover, onerror ```.
```onmouseover```
```
<b onmouseover=alert('Wufff!')>click me!</b>
onerror

<img src="http://url.to.file.which/not.exist" onerror=alert(document.cookie);>


```
### XSS Using Script Via Encoded URI Schemes:

If we need to hide against web application filters we may try to encode string characters, e.g.:``` a=&\#X41 (UTF-8)``` and use it in IMG tags:


```<IMG SRC=j&#X41vascript:alert('test2')>```

There are many different UTF-8 encoding notations that give us even more possibilities.

### XSS Using Code Encoding:

We may encode our script in `base64`and place it in `META` tag. This way we get rid of alert() totally. More information about this method can be found in RFC 2397.
```
<META HTTP-EQUIV="refresh"
CONTENT="0;url=data:text/html;base64,PHNjcmlwdD5hbGVydCgndGVzdDMnKTwvc2NyaXB0Pg">
```




























