---
title: Zenmap Installation Tutorial
description: >-
 An installation guide for installing zenmap tool on Debian systems
author: anorak
date: 2024-09-05 00:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Network Security, Nmap, Zenmap, Linux, Debian, Installation, Cybersecurity]
pin: false
---

## What is Zenmap?

Zenmap is the official Nmap Security Scanner GUI. It is a multi-platform (Linux, Windows, Mac OS X, BSD, etc.) free and open source application which aims to make Nmap easy for beginners to use while providing advanced features for experienced Nmap users.

You can download Zenmap (often packaged with Nmap itself) from the Nmap download page. 

Honestly, if the installation was this easy, I wouldnt have written this blog 

So, IKUZO !!

## Downloading the right package:

  1. Head to the NMap Download [page](https://nmap.org/download):
     
     ![gres](/assets/img/202409/zen1.png){: width="400" height="400" .center}
  
  
  2. Now, Download the latest version of the Zenmap :
  
        ![gres](/assets/img/202409/zen2.png){: width="400" height="400" .center}
  
  3. Then, Open Terminal and go to the downloads directory:
  
        ```bash
        
         cd Downloads/ #change dir to dowload
         ls             #listing the dir to check if the zenmap has been downloaded
        
        ```
  
  4. Run the following commands:
  
        ```bash
        
        sudo apt install alien # allows you to convert Debian packages to RPM packages, and vice versa.
        
        sudo apt-get install RPM #if required
        sudo alien zenmap-7.94-1.noarch.rpm #install the zenmap
        
        ```
  
  5. Install Additional Dependencies:
  
        ```bash 
        
         wget http://archive.ubuntu.com/ubuntu/pool/universe/p/pygtk/python-gtk2_2.24.0-5.1ubuntu2_amd64.deb
        
         wget http://azure.archive.ubuntu.com/ubuntu/pool/universe/p/pygobject-2/python-gobject-2_2.28.6-14ubuntu1_amd64.deb
        
         wget http://security.ubuntu.com/ubuntu/pool/universe/p/pycairo/python-cairo_1.16.2-2ubuntu2_amd64.deb
        ```
  
  
        ```bash
        sudo dpkg -i python-gobject-2_2.28.6-14ubuntu1_amd64.deb
        sudo dpkg -i python-cairo_1.16.2-2ubuntu2_amd64.deb
        sudo dpkg -i python-gtk2_2.24.0-5.1ubuntu2_amd64.deb
        sudo dpkg -i zenmap_7.94-2_all.deb
        
        
        
        ```
  
  6. Result:
  
       If you have followed all the steps which were mentioned above properly, you will be greeted with zenmap gui once you run
       ```
       sudo zenmap
       ```
       ![gres](/assets/img/202409/zen3.png){: width="400" height="400" .center}
  
  7. Uninstallation:
  
      If you are bored with the tool ( I can assure u that you wont be ), just uninstall it by:
      ```
      sudo apt remove Zenmap
      ```

  




