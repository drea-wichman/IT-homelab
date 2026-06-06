# Active Directory Setup Guide

## Part 1: Building the Domain Controller (February 21, 2026)  

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

**Notes:** The LAYERS. A "forest" is the top-level in the AD structure. The "domain" (lab.local) lives inside the forest. In bigger companies, you might have multiple domains in one forest.  
**Example - Forest: Drea's Coffee Company  
**Domains: corporate.dreascoffee.com, retail.dreascoffee.com, roastery.dreascoffee.com  
**Users: jane@corporate.dreascoffee.com (HR), sarah@retail.dreascoffee.com (barista)

Server automatically restarted after.

### 5. Verifying Active Directory Works

**Opened Active Directory Users and Computers** (Server Manager → Tools)

**Saw the domain structure**

**Created a test user: Jane Doe**
- Username: jdoe
- Password: Password1!
- Unchecked "User must change password at next logon"

Confirmed Jane appeared in the Users folder!

<img width="1044" height="864" alt="jane doe" src="https://github.com/user-attachments/assets/ad5d6c64-39ff-4bdd-8529-dfe395f0775e" />



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
- Windows Server EXISTS and is very different from Windows 10/11, designed for managing networks not personal use
- The point of all this is to role play IT setups without needing multiple physical computers

## Resources Used

- Microsoft Evaluation Center (for Windows Server 2022 ISO)
- https://www.virtualbox.org/wiki/Documentation
- Brave search engine, Youtube for tutorials, Reddit for tutorials, Claude for definitions and concept explanation and help basic Github formatting (still getting the hang of it)









        
## Part 2: Windows 10 Client & Domain Join (February 22, 2026)

## Overview
I created a Windows 10 VM to role play an employee's computer (Jane Doe) on a corporate network.

## Installation Steps

### 1. Windows 10 VM Setup

**Created the VM in VirtualBox:**
- Downloaded Windows 10 ISO using Microsoft's Media Creation Tool online
- Allocated 4GB RAM and 50GB storage
- Installed Windows 10 Pro
- Created local account "TestUser" during setup just to get through installation

**Notes:** Must use Pro not Home version because Home cannot access domains. For the Media Creation Tool I selected "I don't have a product key" during install which runs it as trial which is fine for lab. Also it says burn to CD. You don't have to burn to CD.

### 2. Initial Network Configuration

**Set DNS to point to DC01:**
- Network Settings → Ethernet → Properties → IPv4
- Changed DNS from automatic to manual: 192.168.1.10 (DC01's IP address)

**Notes:** Windows 10 needs to know where to find the domain controller. Setting DNS to DC01's IP tells it where to look when trying to join the domain.

### 3. Network Configuration Nightmare

**First attempt to join failed** - Windows 10 couldn't find lab.local at all. Error: "domain controller could not be contacted"

**Problem:** Both VMs were on NAT networking
- NAT isolates each VM but I needed them to talk to each other

**Solution:** Changed both ServerLab and Windows10-Client to Bridged Adapter networking
- Bridged Adapter puts VMs on same network as host computer, restarted both VMs
- Tried to join again and still failed

**Troubleshooting:**
- Command Prompt → `ping 192.168.1.10` → Successful, got replies
- Command Prompt → `ping lab.local` → Fail, "Could not find host"
- Command Prompt → `nslookup lab.local` → Returned "Server Unknown" but did show IP addresses 192.168.1.10 and IPv6
- Command Prompt → `ipconfig /flushdns` → Cleared cache
- Command Prompt → `ping dc01.lab.local` → Fail
- Command Prompt → `ipconfig /all` → Confirmed DNS server was set to 192.168.1.10

This was confusing me because DNS was set to 192.168.1.10 and that server was reachable. IP worked but name didn't.

**Checked DNS on DC01:**
- Opened DNS Manager on ServerLab
- lab.local zone existed and DNS service was running so(?)

**The actual problem:** Apparently a "DNS resolution timing issue". Reddit explained it this way: "Even though DC01's DNS was configured correctly Windows 10 couldn't resolve domain name through it. Can happen when DNS caching gets confused or when DNS service hasn't fully propagated after network changes."

### 4. The Hosts File Solution

**Used Windows hosts file as workaround to bypass DNS:**
- Opened Notepad as Administrator
- File → Open → `C:\Windows\System32\drivers\etc`
- Changed filter to "All Files" → Opened `hosts` file
- Added line at bottom: `192.168.1.10    lab.local dc01.lab.local` → Save

**Notes:** In my mind the hosts file is like a phone book that Windows checks in order to ask DNS. Basically I bypassed the broken DNS lookup by giving Windows a direct map. "When you see lab.local, go directly to 192.168.1.10 and don't bother asking DNS." Not permanent solution for real environments but fine for this I hope.

### 5. Joining the Domain

Tried joining one more time after hosts file fix:
- System → About → Advanced System Settings → Computer Name → Change
- Selected Domain  → Typed: lab.local  →  OK

**This time prompted for credentials!**

**Entered:**
- Username: Administrator
- Password: Password1!

Got success message finally: **"Welcome to the lab.local domain"** 

Computer prompted to restart. Rebooted and success.

### 6. Testing Domain Login

After restart login screen looked different:
- Showed domain user field instead of TestUser
- "Other user" option appeared in bottom left
- Logged in as Jane Doe

**IT WORKED!** Could see "jdoe" in Start menu showing I'm in there.

**Notes:** In real world, help desk creates your account in AD once and it works on every company computer, new laptops and everything else included, just log into domain. No separate accounts needed on each machine and password resets update everywhere instantly.


<img width="790" height="610" alt="2 jane doe yay" src="https://github.com/user-attachments/assets/d40ac200-4d83-4091-b148-bdecc05dcff2" />



## Challenges & Solutions

**Challenge:** Windows 10 couldn't find domain controller  
**Solution:** Changed network from NAT to Bridged Adapter so VMs could communicate

**Challenge:** Domain join failed even with DNS set correctly - ping by IP worked but ping by name failed  
**Solution:** Added manual entry to hosts file in Notepad directing lab.local to 192.168.1.10 bypassing DNS  

**Challenge:** Confusing why hosts file was necessary when DNS was configured correctly  
**Solution:** DNS has timing/caching issues after network changes. Hosts file provides direct mapping Windows checks before querying DNS giving reliable fallback. Still processing this.

## Key Takeaways

- Network configuration needs to be perfect
- No DNS, no AD
- Hosts file useful for temp troubleshooting
