--- 
title: Hacker's Roadmap 101
description: >-
  An Ethical Hackers Guide for Getting from Beginner to Professional
author: anorak
date: 2025-04-26 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity, Database, Ethical Hacking, Penetration Testing, Web Security]
pin: False
---  
# A Cybersecurity Curriculum for Builders Who Want to Learn to Break

When I first decided to learn about cybersecurity, I had no idea where to start. I just knew that I really loved solving puzzles and that *Hackers* was one of the best movies of all time. I set up a curriculum for myself by talking to anyone I could find who worked in security and looking back on it now I’m pretty proud of what I put together. But I also wish I’d found out about some resources sooner and others later, so I put together this revised curriculum for programmers who already know how to build things and want to learn how to break them.

## 1. Getting started

### Hackthis.co.uk  
[Hakcthis.co.uk](https://hackthis.co.uk) is an accessible gamified hacking intro.

This is a great first resource to check out. The main levels are pretty straight forward and provide a quick way to find out if you even like offensive work. The more advanced levels will help you find out how you react to struggling with a puzzle. The crypto levels in particular get interesting. Feel free to check the hints and read the forums if you get stuck. If you like this format and finish all the levels, check out [hackthissite.org](https://hackthissite.org). It’s older and less attractive, but it will have some different challenges to try.

### Microcorruption.com  
[Microcorruption.com](https://microcorruption.com) offers a terrific way to learn about assembly hacking and buffer overflows.

This is the best way to learn about assembly and buffer overflows I’ve ever seen. The scenario is interesting, the UI is amazing, and the levels are challenging. To be honest, I find I don’t use much assembly reversing or buffer overflows on the job, but it’s a really great way to test your passion. Hopefully this will get you to a point where you hit a wall, have no idea what to do, and wind up staring at the same few lines of code for hours. If you still feel excited about solving the puzzle at that point, then cybersecurity might really be for you.

### Vulnhub.com  
[Vulnhub](https://vulnhub.com) will expose you to tools of the trade like Nmap.

Vulnhub is a great thing to check out early because it will teach you how to use some of the security tools that are used in the field like Burp Suite, Nmap, and Hydra. Some of the virtual machines are a pain to set up, but that too can be a good learning experience. The other great thing about Vulnhub is that there are a ton of write ups to follow along with. For a little bit of shameless self-promotion, you can find a Vulnhub walk through I wrote for absolute beginners here.

## 2. Getting serious

### The Web Application Hacker’s Handbook  
There’s a copy of *The Web Application Hackers Handbook* on my desk at Praetorian and another one at home.

This is a textbook, not a website. And anyone who is seriously thinking about working in web application security should read it. It goes through all of the major attack categories that you will see in the wild like attacking authentication, stored vs reflective cross-site scripting, and SQL injection. And more than that it will teach you how to remediate each of those attack categories. Half of my job at Praetorian is helping clients fix the security vulnerabilities I expose, and this book has been extremely helpful for that. Along the way it will also teach you a lot about using BurpSuite, which is an invaluable application hacking tool. It’s almost a thousand pages, and you should read it cover to cover.

### Pentesterlabs.com  
[Pentesterlabs](https://pentesterlab.com) offers a lot of short lessons on specific attack paths.

It’s the first paid site on this list, but with a pretty low monthly fee it’s well worth its price tag. Some of the exercises will be easy, especially if you have already started on the resources above. But you are sure to learn a few new tricks that will come in handy someday. Think of it like hacking drills.

### Hackthebox.com  
While [Hackthebox](https://hackthebox.com) is free, you’ll have to hack your way in first.

Hack the box has a ton of high quality, free, vulnerable machines. It’s way easier to use than Vulnhub, but there are also no walk throughs. It’s a good place to put what you have learned to the test. Can you use what you know about network and application security to hack a machine without help? Can you wrestle with the same problem for days and still enjoy the work? This is another great place to test your commitment to learning offensive cybersecurity.

## 3. Getting hired

### eLearnSecurity  
If you are really interested in working as a professional security engineer or penetration tester, then [eLearnSecurity](https://elearnsecurity.com) is a great place to solidify your baseline knowledge.

The Penetration Testing Professional course is a valuable course and is priced accordingly, but if you’re serious about security, it’s worth it. The videos and PDF instructions are clear and easy to follow along with. The curriculum hits key areas like buffer overflows, network security, web application security, and even Wi-Fi security. And most importantly, the labs are great. They are easy to use, have clear instructions, are very hands on, and if you get stuck, they all have solutions included (but don’t look at those unless you really have to).

### Offensive Security Certified Professional  
The [OSCP](https://www.offensive-security.com/pwk-oscp/) is pretty much the only certification that anyone actually looks for in the field.

If you want to be a professional hacker, then at some point you should look at getting your OSCP. The lab instructions will not hold your hand like the eLearnSecurity ones, but that’s the idea. It’s a chance to prove to yourself and potential employers that you are ready to go off on your own and figure out whatever you need to know to get the job done. The OSCP lab is expansive and challenging. Some of the machines are famously brutal. This is, again, a great way to test your resolve. Do you really want to spend your time enumerating every inch of attack surface, scouring the web for exploits, privilege escalation techniques, and post exploitation methodologies? If so, then you will have a bright future in cybersecurity. If you commit to the OSCP then my biggest tips are to go through all the lab material, read the walk through for Alpha on the forums, and look up OSCP methodologies. And of course, try harder!

### Praetorian Tech Challenges  
The [Praetorian Tech Challenges](https://www.praetorian.com) cover a broad range of cybersecurity material. From brute forcing, to cryptography, to machine learning.

If you have made it this far and now know that you want to work as a security engineer, give these a shot. Some are harder than others and I encourage you to pick ones that will challenge you. Each one will teach you a lot about a different area, and if you solve any of them you will be rewarded with a hash that will grant you an interview with Praetorian. This was by far the best interview experience I ever had, and it played a big part in my decision to join Praetorian as a full time security engineer. Good luck!

## Conclusions

There are a lot of resources here, don’t feel like you need to do them all or that you need to do them in order. Find what works for you. When I was studying full time I generally spent the morning reading textbooks like *The Web Application Hackers Handbook*, *Black Hat Python*, and *Hacking: The Art of Exploitation* (also great). Then I would spend most of my day working on the more challenging resources like microcorruption and hackthebox, and once I was burnt out in the evening I’d go through some of the easier material like hackthis.co.uk and pentesterlabs. Whatever you do, don’t stop challenging yourself, and don’t give up!
