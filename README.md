# IT Home Lab Setup

## Overview
Hands-on home lab environment for IT fundamentals, system administration, and security practice.
Will update as I build it out, more often now that A+ is done.

## What I've Built

- ✅ Windows Server 2022 domain controller with Active Directory
- ✅ Dual-boot Windows 10 Pro + Ubuntu 24.04
- ✅ Encrypted USB drive with VeraCrypt
- ✅ Full disk encryption on all personal devices
- 🔄 Phase 2 Active Directory — in progress
- 🔄 Ubuntu practice — in progress

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
- Ancient MacBook Air running Ubuntu — Linux command line practice for coffee shop study days

### Software

**Virtualization Platform:**
- Oracle VirtualBox 7.0
- Eventually want to try VMware Workstation Player

## Phase 1: Environment Setup ✅

- Installed and configured VirtualBox
- Created Windows Server 2022 VM
- Set up Windows 10 as client machine
- Installed Ubuntu 24.04 via dual boot

## Phase 2: Active Directory Lab 🔄

(That's all I have planned so far. The rest is TBD...)

## Progress Log

*Will update as I build it out and practice*

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
