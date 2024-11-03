---
layout: default
title: "Initial lab setup"
date: 2024-11-03
---

# Lab Setup
* VMware Workstation Pro
* Windows Server 2025
* Windows 10 and Windows 11 (Clients)

# Reasons for choices

## Hypervisor
I have chosen to use VMware Workstation to host my virtual machines as it is what I am most familiar with and it was already installed on my system.
## Operating Systems
### Server
I chose Windows Server 2025 as for AD/DNS and DHCP setup, everything is identical to Windows Server 2022 alongside additional security measures
### Client
Even though the client SKUs of Windows 10 reach end of support in 2025, the LTSC will continue to be supported up until the start of 2027.

# Installing AD/DNS/DHCP
### Issues encountered:
* After setting up a static IP, I had an issue where I could not connect to the external internet. 
 This was because I had forgotten to insert VMware Workstation's default gateway, which in my case was 192.168.31.2
* Changing the server name after joining a domain prevented me from logging in. As I was unable to fix it, I had to reinstall Windows Server.

### Connecting clients to domain
Once AD/DNS and DHCP roles were installed and set up, connecting Windows 10 and 11 to the domain was easy and no issues were encountered

![Basic](/assets/images/BasicSetup.png)
Windows 10 and Windows 11 clients connected to Windows Server 2025 domain.


# Next Steps
Now that the initial setup of AD has been completed. My goals for the next week are...
* Mimic group and user structure found in companies
* Create group policies for said groups
* Learn how to do both via PowerShell



[Back to Home]({{ "/" | relative_url }})
