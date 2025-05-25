---
title: Is ToR Safe and Anonymous?
description: >-
  Deepdive into working and methodology of ToR Networks
author: anorak
date: 2024-10-01 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [TOR]
pin: false
---

Most of you know that the The Onion Network or ToR is the backbone of the Dark Web. First developed by the US Navy to hide their online activity, it is now an open-source project available to all via the ToR Project . Many people believe that their activities’ online through the ToR Network are safe and anonymous. In fact, most people make the assumption they are anonymous while using the ToR network and, as a result, make fatal mistakes

This week a German publication reported that German law enforcement had found a way to de-anonymize people on the ToR network. This information received a fair amount of press, but it is really old news. Over a decade ago, the National Security Administration (NSA) in the US, pioneered this de-anonymizing technique 

It’s not easy and it can be expensive but it is possible to determine your identity if you traverse the ToR network. This applies if you use ToR to visit a regular Internet site, but NOT if you remain within the ToR network. The key to de-anonymizing you is correlating your packet data between entry and exit nodes.

![img](/assets/img/202410/tor.webp){: width="600" height="600" .left}

For years, websites and intelligence agencies have used the cookies embedded in your browser to identify you, your personal attributes, and the sites you visit. Knowing this, users seeking anonymity will disable cookie collection by their browser and hope to remain anonymous. Unfortunately, a newer technique, known as browser fingerprinting, may be even more nefarious and accurate. The intelligence agencies and others can use browser fingerprinting among other things to de-anonymize you.

Whichever browser you use, it leaves a fingerprint of your identity. The law enforcement agencies simply correlate your traffic fingerprint with the traffic leaving the ToR network. If they are the same, they have identified your unique traffic. The next step is to linking your traffic and to your identity. That is the easy part.

To better understand this de-anonymizing technique, let’s take a closer look at browser fingerprinting. We can divide these browser fingerprints into seven (7) primary categories and each of these can be further divided in finer sub-categories yielding over 50 variables to de-anonymize you. With 50 variables and at a minimum of two options (on/off), the number of unique possibilities is 1,125,899,906,842,624. Since there are only  7,500,000,000B people on earth, this system has over 150,000 potential unique possibilities per person. More than enough to de-anonymize nearly everyone.

Here is a fraction of the fingerprint elements from default browser in Kali.

![img](/assets/img/202410/tor2.webp){: width="600" height="600" .center}



## What is Browser Fingerprinting?

What is browser fingerprinting? This is the practice of identifying a multitude of parameters that your browser communicates to a website to assist the website with providing you with a best web experience. There are over 50 variables that your browser communicates and can be used to identify you.

These parameters include:
### 1.  Canvas Fingerprinting

Canvas fingerprinting is a sophisticated web tracking technique that utilizes the HTML5 canvas element to create unique identifiers for users, allowing websites to identify and track visitors without relying on traditional cookies.
#### How Canvas Fingerprinting Works

 -   Rendering Process: When a user visits a webpage that employs canvas fingerprinting, a script draws text or graphics onto an HTML5 canvas element. This process involves specifying various attributes like font size and background color.

-    Data Extraction: The script then uses the Canvas API’s toDataURL method to convert the drawn content into a Base64 encoded string, which represents the pixel data of the canvas.

 -   Hashing: Finally, this string is hashed to create a unique fingerprint that can be stored and used for tracking purposes across different websites.

The uniqueness of the fingerprint arises from variations in how different browsers and operating systems render the same content. Factors such as the user’s graphics hardware and software configurations can lead to slight differences in the resulting image, thereby creating a distinct identifier for each user
### 2. WebGL Fingerprinting

WebGL fingerprinting is a technique used to identify and track users based on the unique characteristics of their device’s graphics hardware through the WebGL API. This method leverages the rendering capabilities of the user’s GPU (Graphics Processing Unit) to create a unique identifier that can persist across different browsing sessions and websites.

 -   WebGL Context Creation: When a user visits a website that utilizes WebGL, the browser first establishes a WebGL context, which allows it to communicate with the GPU and perform graphics rendering tasks.

 -   Graphics Rendering: The website then instructs the browser to render specific graphics, often involving complex 3D shapes or patterns. The rendering process is influenced by various factors, including the user’s graphics card, drivers, and browser implementation.

 -   Data Capture and Hashing: After rendering, the output is captured, and specific characteristics of the image are analyzed. This data is then hashed to create a unique fingerprint that represents the device’s graphical rendering capabilities.

 -   Storage and Tracking: The generated WebGL fingerprint can be stored on the server, allowing the website to recognize returning users by comparing new fingerprints with previously stored ones.

#### Factors Contributing to Uniqueness

The uniqueness of a WebGL fingerprint arises from several factors:

  - Graphics Card Variations: Different GPUs render images in slightly different ways due to inherent differences in hardware design.

 -  Driver Versions: Updates and variations in graphics drivers can alter how graphics are processed.

-    Browser Differences: Each browser may implement WebGL differently, leading to variations in rendering outcomes.

 -   Operating System Influences: The underlying operating system can also affect how graphics are rendered and processed

### 3. Media Device Fingerprinting

Media device fingerprinting is a technique used to uniquely identify and track devices based on the specific characteristics of their media capabilities. This method collects various attributes related to the audio and video hardware and software configurations of a device, allowing for persistent identification even in scenarios where traditional tracking methods, such as cookies, may be blocked or deleted.
#### How Media Device Fingerprinting Works

  -  Data Collection: When a user interacts with a website or application that employs media device fingerprinting, the system gathers information about the device’s media-related features. This can include:

   - Audio and Video Capabilities: Supported codecs, resolutions, and formats.

  -  Device Specifications: Information about the microphone, camera, and speakers.

 - Browser and OS Information: Details about the operating system and web browser being used.

    Unique Identifier Creation: The collected data is processed to create a unique identifier or “fingerprint.” This fingerprint is generated by analyzing the combinations of attributes that are often unique to each device.

    Tracking Mechanism: Once a fingerprint is created, it can be stored on the server. When the same device connects again, the system can recognize it by matching the new fingerprint with previously stored identifiers.

### 4. TLS Fingerprinting

TLS fingerprinting is a technique used to identify and characterize clients based on the information exchanged during the TLS (Transport Layer Security) handshake, specifically the “Client Hello” message. This method leverages the unique attributes of a client’s TLS implementation, allowing for effective identification even when traditional identifiers like cookies are blocked or deleted.
How TLS Fingerprinting Works

  ####  TLS Handshake: The process begins with a TLS handshake, where the client requests a secure connection to the server. This handshake involves several steps, including the exchange of cryptographic parameters.

   - Client Hello Message: During this handshake, the client sends a “Client Hello” message that contains crucial information, such as:

      -  Supported Cipher Suites: A list of encryption algorithms that the client supports, presented in order of preference.

     -   TLS Version: The version of the TLS protocol that the client wishes to use.

      -  Extensions: Optional parameters that can enhance or modify the connection, such as Server Name Indication (SNI).

   - Fingerprint Generation: By analyzing the fields in the Client Hello message, a unique fingerprint can be created. This fingerprint reflects not only the client’s software and hardware configurations but also its specific TLS library (e.g., NSS for Firefox or BoringSSL for Chrome) and its supported cipher suites.

   - Hashing: The collected data can be hashed to create a compact representation of the fingerprint, often using formats like JA3, which generates an MD5 hash from specific fields in the Client Hello message.

###  5. Font Fingerprinting

Font fingerprinting is a tracking technique that identifies and monitors users based on the specific fonts installed on their devices. This method exploits the uniqueness of font combinations across different systems to create a distinctive identifier, which can be used to track users across various websites and sessions.

#### How Font Fingerprinting Works

  - Detection of Installed Fonts: When a user visits a website, scripts run in the background to detect which fonts are available on their device. This can be accomplished through several methods:

   -  JavaScript and CSS: Webpages can dynamically test for fonts by measuring the dimensions of rendered text in various fonts.

  -  Canvas API: The HTML5 Canvas API can render text with different fonts and measure the resulting sizes to determine font availability.

   -  Font Enumeration: This straightforward approach uses JavaScript to request a list of installed fonts from the browser.

   Creating a Unique Identifier: The information gathered about installed fonts is analyzed and hashed to generate a unique fingerprint. This fingerprint reflects the specific combination of fonts present on the user’s device, which is often unique enough to differentiate users effectively.

  Tracking Mechanism: Once created, this fingerprint can be stored on servers, allowing websites to recognize returning users without relying on cookies or other traditional tracking methods.

### 6. Mobile Device Fingerprinting

Mobile device fingerprinting is a specific type of device fingerprinting that focuses on collecting data about mobile devices, such as smartphones and tablets, to create a unique identifier for each device. This process involves gathering various attributes related to the device’s hardware and software configurations, allowing for persistent tracking and identification even when traditional methods like cookies are not available.
#### How Mobile Device Fingerprinting Works

  -  Data Collection: Mobile device fingerprinting collects a wide range of data points, including:

  -  Operating System: Information about the mobile OS (e.g., iOS, Android).

 -    Screen Resolution: The dimensions of the device’s display.

 -    Battery Level: Current battery status, which can vary between devices.

 -    GPS Location: Geolocation data if location services are enabled.

  -   Installed Applications: A list of apps currently installed on the device.

  -    Hardware Specifications: Details such as processor type, RAM, and other unique identifiers.

   Creating a Unique Identifier: The collected data points are combined to form a unique identifier or “fingerprint” for the mobile device. This fingerprint is statistically unlikely to be duplicated by another device due to the unique combination of attributes.

   Tracking Mechanism: Once generated, this fingerprint can be stored on servers, allowing companies to recognize returning devices across different applications and websites. This capability enables continuous tracking of user behavior.

### 7. Audio Fingerprinting

Audio fingerprinting is a technology that allows for the identification and tracking of audio content. It works by creating a unique digital signature or “fingerprint” for each audio recording based on its acoustic characteristics. These fingerprints can then be used to identify the audio in various applications. The process of audio fingerprinting involves:

   - Extracting perceptual features from the audio signal, such as the waveform, spectral envelope, pitch, harmonics, and overtones.

   - Condensing these features into a compact digital summary called an acoustic fingerprint.

  -  Comparing this fingerprint against a database of pre-computed fingerprints to identify the audio.

####  key attributes of audio fingerprints:

  -  They are robust to audio degradations like compression, noise, and transmission artifacts.

  -  They tolerate small variations in the audio that don’t affect human perception.

  -  They allow for quick identification of audio by comparing against large databases.

### Summary

Many people have used ToR and VPN’s to obscure their identity online. Although these services will obscure your IP addresses, intelligence and commercial interests have developed an even more effective way of identifying you through browser fingerprinting. This works even while cookies are disabled.

Be careful while using a VPN or ToR as they only hide your IP address. Far more data is being hauled around the Internet in your browser that can and will be used to de-anonymize you.
