---
title: Hypervisors Explained
description: >-
 Everything you need to know about hypervisors is here
author: anorak
date: 2024-08-02 18:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Virtualization, Hypervisor,  Cybersecurity]
pin: false
---

## What is an Hypervisor?
I am sure most of you would have already used a hypervisor before.If you haven't already figured out what it is,all the Virtual Machines that you might have used comes under this category.

- A hypervisor is hardware, software, or firmware capable of creating virtual machines and then managing and allocating resources to them.
- Virtual machines are machines set up to use the resources of the host machine. You can divide these resources as many times as you like to accommodate the necessary virtual machine “guests.”
- However not all the Virtual machines run on a OS , few of them like Qubes OS runs on baremetal and used Zen Hyervisor to handle resources.Lets now understand what are the different type of hypervisor.

 
## How does it Work?

- Hypervisors and collections of virtual machines are used for numerous different tasks in a business setting, including data replication, server consolidation, desktop virtualization, and cloud computing.
- Typically, when you want to replicate a virtual machine, you have to replicate its entire volume manually. Using a hypervisor, you can simply choose which virtual machines and parts you want replicated, and it will perform the process for you.
- Desktop virtualization is useful when you want to use a piece of software compatible with one operating system, such as Windows, but you have another operating system, such as Linux or Mac OS, on your machine.
- With a hypervisor, you can set up a Windows virtual machine to run the software without having to change operating systems.

## Types of Hypervisor:
The Hypervisors are classified on the platform they work upon,they can be classified as:
1. [Type I Hypervisor](#type-i-hypervisor)
2. [Type II Hypervisor](#type-ii-hypervisor)

![types](/assets/img/202408/hypervisor1.png)

### Type I Hypervisor

- Also known as bare metal hypervisor is installed directly on the hardware of your machine, whereas a hosted hypervisor is installed on your operating system.
- A Type 1 hypervisor is a layer of software installed directly on top of a physical server and its underlying hardware. Since no other software runs between the hardware and the hypervisor, it is also called the bare-metal hypervisor.

- This hypervisor type provides excellent performance and stability since it does not run inside Windows or any other operating system. Instead, it is a simple operating system designed to run virtual machines. The physical machine the hypervisor runs on serves virtualization purposes only. 
- Bare metal hypervisors are typically faster and more efficient because they have direct access to the underlying hardware and don’t need to go through the operating system layer. Since they don’t have to compete with other applications or the OS, they can take all the available physical hardware power and allocate it to virtual machines. They also tend to be more secure, because, without an operating system on the host, less attack surface is available for malicious intruders.
- They’re often used for testing and development purposes, as they can run on the OS to try out new programs or features without affecting the host OS.
- Few of the examples include VMware vSphere with ESX/ESXi, KVM, Hyper-V, Oracle VM.


### Type II Hypervisor

- Type 2 hypervisors run inside the physical host machine's operating system, which is why they are called hosted hypervisors. Unlike bare-metal hypervisors that run directly on the hardware, hosted hypervisors have one software layer in between. The system with a hosted hypervisor contains:

    - A physical machine.
    - An operating system installed on the hardware (Windows, Linux, macOS).
    - A type 2 hypervisor software within that operating system.
    - Guest virtual machine instances.
- What makes them convenient is that they do not need a management console on another system to set up and manage virtual machines. Everything is performed on the server with the hypervisor installed, and virtual machines launch in a standard OS window.

- Hosted hypervisors also act as management consoles for virtual machines. Any task can be performed using the built-in functionalities.

## How is it useful in the field of Cybersecurity:

- Me being a Security Researcher, apart from playing traditional CTF's on web, I also simulate sandboxes by spawning vulnerable VM's to conduct various cyberattacks in an isolated environment to test the severity and other factors of the attack.
- I will share how to build your own isolated environment for penetration testing in some other article.































