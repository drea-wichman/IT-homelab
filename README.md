# IT Home Lab Setup

## Overview
Building a home lab environment to gain hands-on experience with IT fundamentals

## My Goals
- Work with Windows Server and Active Directory(!!!)
- Practice hardware troubleshooting and component installation
- Configure network settings and connectivity
- Manage multiple OS and virtual machines
- Simulate IT support scenarios

## Lab Components

### Hardware

**Desktop Computer:**
- **Model:** HP EliteDesk 800 G3 Desktop Mini (eBay refurb)
- **CPU:** Intel Core i7-7700 (7th Gen, Quad-Core, 3.6GHz)
- **RAM:** 32GB DDR4
- **Storage:** 256GB SSD
- **Graphics:** Intel HD Graphics 630
- **Operating System:** Windows 10 Pro
- **Ports:** 
  - 1x USB Type-C
  - 6x USB 3.1
  - 2x DisplayPort
  - 1x HDMI
  - 1x Ethernet (RJ-45)

**Monitor:**
- **Model:** LG 24G411A-B Ultragear
- **Display:** 24-inch Full HD IPS (1920x1080)
- **Refresh Rate:** 144Hz

**Additional Equipment:**
- Ancient MacBook Air running Ubuntu - Linux command line practice for coffee shop study days

  
### Software

**Virtualization Platform:**
- Oracle VirtualBox 7.0 (to start I think)
- Eventually try VMware Workstation Player

### Phase 1: Environment Setup + Intentions
- Install and configure VirtualBox
- Create Windows Server 2022 VM - to get Active Directory practice
- Set up Windows 10  - to pretend it's a help desk client's computer
- Install Ubuntu 22.04 Desktop VM - to learn Linux


### Phase 2: Active Directory Lab


  (That's all I have planned so far. The rest is TBD...)


## Progress Log
*Will update as I build it out and practice, more often after I've completed A+ exam*

### February 21, 2026
- Downloaded/installed VirtualBox 7.0 on HP EliteDesk
- Troubleshot missing Visual C++ 2019 and resolved
- Downloaded/installed Windows Server 2022
- :) Created 1st VM: Windows Server 2022
- Installed Windows Server 2022 with desktop experience
- Configured static IP: 192.168.1.10
- Installed Active Directory Domain Services role
- Promoted server to Domain Controller
- Created domain: lab.local and first user: Jane Doe (jdoe@lab.local)
