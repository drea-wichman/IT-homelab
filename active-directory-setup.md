# Active Directory Setup Guide

## Overview
This documents my first time setting up Active Directory Domain Services on Windows Server 2022 in my home lab environment.

## Environment

**Hypervisor:** Oracle VirtualBox 7.0  
**Host OS:** Windows 10 Pro  
**VM Configuration:**
- OS: Windows Server 2022 Standard Evaluation (Desktop Experience)
- RAM: 4GB, Storage: 50GB, vCPUs: 2

## What is Active Directory?

Basically Active Directory is how companies manage user accounts and computers across their network in one place nstead of creating separate logins on every computer. IT uses it to control access, passwords, and security policies from one central place.

## Installation Steps

### 1. Initial VM Setup

**Created the VM in VirtualBox:**
- Rookie mistake: Trekked Brooklyn in the snow to buy a DisplayPort cable because I wasn't paying attention and my HP Elitedesk doesn't have HDMI port to connect to monitor
- Downloaded Windows Server 2022 from Microsoft
- Allocated 4GB RAM and 50GB storage
- Installed Windows Server with Desktop Experience option

**Problem:** VirtualBox installation failed initially  
**Solution:** Had to install Microsoft Visual C++ 2019 first, then redo VirtualBox installation

**Notes:** I had to read the VB installation log file to see why it failed. I learned log files are listed in consecutive order. Very simple thing but I expected most recent events to be at the top. I also didn't know AD would only work on Windows server vs. Windows 10/11. I never knew they were completely separate OS, I just thought VB would be additional software slapped on top of host windows 10/11. Also had to look up why "Desktop Experience"? Because it has the normal Windows GUI and without it I would only get the command line which I'm not ready for. Datacenter option would've been overkill apparently.

### 2. Server Configuration

**Set static IP address:**
- IP: 192.168.1.10
- Subnet: 255.255.255.0
- DNS: 127.0.0.1 - points to itself since it'll be the DNS server ("Where's the DNS server?" "I AM the DNS server")


**Notes:** I needed to use a consistent IP addresses for this Domain Controller so other computers can always find it. Other VMs I set up won't have static IP because they are role playing "employee computers".

### 3. Installing Active Directory Domain Services

**In Server Manager:**
1. Manage → Add Roles and Features → "Active Directory Domain Services" role → clicked through wizard and installed

**Got warnings during prerequisites check** - had to look up online but these were normal for a new lab environment they said

### 4. Promoting to Domain Controller

After AD DS installed → clicked notification flag → "Promote this server to a domain controller"

**Configuration choices:**
- Deployment: Add a new forest (creating brand new AD from scratch)
- Root domain name: lab.local
- Forest/Domain functional level: Windows Server 2016
- Enabled DNS server and Global Catalog

**What I learned:** The LAYERS. A "forest" is the top-level in the AD structure. The "domain" (lab.local) lives inside the forest. In bigger companies, you might have multiple domains in one forest.  
**Example - Forest: Drea's Coffee Company  
**Domains: corporate.dreascoffee.com, retail.dreascoffee.com, roastery.dreascoffee.com  
**Users: jane@corporate.dreascoffee.com (HR), sarah@retail.dreascoffee.com (barista)

Server automatically restarted after.

### 5. Verifying Active Directory Works

**Opened Active Directory Users and Computers** (Server Manager → Tools)

**Saw the domain structure**

**Created a test user: Jane Doe**
- Username: jdoe
- Password
- Unchecked "User must change password at next logon"

Confirmed Jane appeared in the Users folder!

## What's Next

- Create a Windows 10 VM (employee role)
- Join it to the lab.local domain
- Test logging in as Jane Doe from the Windows 10 machine
- Create more users and groups

## Challenges & Solutions 

**Challenge:** VirtualBox wouldn't install  
**Solution:** Installed Visual C++ 2019 first, rebooted, retried

**Challenge:** Understanding the difference between forest, domain, and domain controller  
**Solution:** Researched and learned: forest = whole AD setup, domain = network within forest, DC = server that runs AD

**Challenge:** DNS delegation warning during AD setup  
**Solution:** Normal for iso lab

## Key Takeaways

- Domain Controllers need static IPs so they're always reachable
- Windows Server EXISTS and is very different from Windows 10/11 - it's designed for managing networks not personal use
- The point of all this is to role play IT setups without needing multiple physical computers

## Resources Used

- Microsoft Evaluation Center (for Windows Server 2022 ISO)
- https://www.virtualbox.org/wiki/Documentation
- Brave search engine, Youtube for tutorials, Reddit for tutorials, Claude for definitions and concept explanation and help basic Github formatting (still getting the hang of it)
- Trial and plent of error
