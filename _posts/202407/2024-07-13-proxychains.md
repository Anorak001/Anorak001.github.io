---
title: Stay Anonymous by using Proxychains in KaliLinux
description: >-
 A detailed tutorial on how to configure the proxychains in Kali Linux
author: anorak
date: 2024-07-13 00:00:00 +0530
categories: [GUIDE,LINUX]
tags: [Linux, Cybersecurity]
pin: false
---
# Staying Anonymous with ProxyChains in Kali Linux:
- Being anonymous on the internet or while carrying out an attack is one of the most important characteristics of a hacker. There are several ways to achieve this anonymity, one of which is by configuring proxy chains.
  It is an excellent way to remain anonymous while browsing the internet or conducting attacks. To achieve this anonymity, we will configure ProxyChains in Kali Linux in this article. 

## What are Proxy chains?
- Proxychains are essentially a combination of proxies that reroute a TCP connection to any server using different protocols, such as HTTP, HTTPS, SOCK4 & SOCKS5, making the connection secure and making it nearly impossible for an observer to figure out what you are looking for up online and where you are. Through a very secure chain of proxies, ProxyChains serves as an intermediary between the source IP and the endpoint IP, concealing your identity.
- Most people mistakenly believe that VPNs are safer than ProxyChains, however, the reality is that DNS leaks are more common in VPNs, making them less secure than ProxyChains.

## Steps to Configure the Proxychain:

### Checking for dependencies:
   
  - The Configuration does not really require any large module or packages except *Proxychains* package.It comes preinstalled with KaliLinux.
  - However, if you are using any other Debian Flavours like Ubuntu, You can install it by running the following commmand:
    ```sudo apt install proxychains```

### Configuration:
   
   1.  Open the terminal
   2.  Firstly,we will navigate to the target directory where the ***proxychains4.conf*** is stored by running the command:
   3.  Make a backup of the file by renaming ***proxychains4.conf** to ***proxychains4.conf.backup**
    ``` cd /etc/ ```

   5.  Open the configuratiion by using any text editor.I'm using the default text editor in Kali Linux (GEDIT).
      ``` gedit proxychains4.conf```

  6.  In the Recently updated Kali Linux Distro[**2024.1**], The Configuration is being stored in *Proxychains4.conf* whereas in the previous versions, the configurations were stored under the name *Proxychains.conf*.
      
  7.  The file when opened will Look like this:
      ![gedit window](/assets/img/202407/geditwindow.jpg){: width="1100" height="549" .w-75 .normal}
  
  8.  Uncomment the dynamic chain and comment out the strict chain by removing and replacing the # sign.
      ![gedit window](/assets/img/202407/chain.png){: width="1100" height="549" .w-75 .normal}
        
      - **dynamic_chain** – Will create a new chain of proxies whenever a new connection is opened.
      - **strict_chain** – Will only use the last chained proxy while opening every new connection.
        So, in this case, we make the dynamic chain the default setting.
     
  9. Now search the web for websites that provide proxies, in this case, [proxyscrape](https://proxyscrape.com/free-proxy-list),
       and select the protocol you want to use; HTTP and HTTPS were used for demonstration purposes, but SOCK5 or SOCKS 4 can also be used. Make as many proxies as you like.
  
        ![proxyscrape](/assets/img/202407/proxyscrape.png){: width="1100" height="549" .w-75 .normal}
  
  10. Scroll down to the bottom of the file:
          ![end](/assets/img/202407/bottom.jpg){: width="1100" height="549" .w-75 .normal}
  
  11. Copy and paste the proxies in the format shown in the image:
      
       ![List](/assets/img/202407/copynpaste.jpg){: width="1100" height="549" .w-75 .normal}
  13. Save and close the file. Check if the connection is successful by using **ping** commmand:

        ```ping www.google.com```
  
  Done! Your system has now been set up to use multiple proxies. The proxies will be used, but to be safe, restart the system.
  

### Troubleshooting:

- The setup should just work fine.However if you are unable to connect to internet,revert the changes done by deleting the file and renaming the backupfile.

## Practical Use Cases:
- Few of the cases where you can use the proxychains are:
  1. Accessing Blocked Content
  2. Avoiding Transfer Rate Limit to particular IP
  3. Accessing Blocked Content
  4. Security Testing
  5. Defensive Strategy to Hide the Base IP

Hence,By following the above steps, you can configure your very own proxychains to stay anonymous and browse the web.

## Ongoing Project:
- My friends and I are developing a proxy chain automation web app, but we're currently looking for a skilled front-end developer to join our team. If you're interested, please get in touch!









