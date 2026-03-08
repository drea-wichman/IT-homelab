# Partitioning & Encryption

## Overview
Hands-on lab covering disk partitioning, dual-boot OS installation, and USB encryption

## Homelab Environment
- **Primary OS:** Windows 10 Pro
- **Secondary OS:** Ubuntu 24.04

- - -

## Part 1: Disk Partitioning & Dual Boot

### Goal
Install Ubuntu alongside Windows 10 Pro on my EliteDesk homelab to create a dual-boot environment

### Steps
1. Opened Disk Management on Windows and shrunk C: to create unallocated space
2. Downloaded Ubuntu ISO from browser
3. Downloaded Rufus from browser and used this to make bootable USB
4. BitLocker was blocking Ubuntu installer so I eventually used `manage-bde -off C:` in PowerShell to resolve
5. Restarted and spammed F9 to boot from USB
6. Installed Ubuntu into the unallocated 50GB partition
7. Ran `sudo update-grub` in Ubuntu terminal to detect Windows and add it to the GRUB menu
8. Wiped the bootable USB and reformatted as NTFS for reuse


### Final Partition Layout
| EFI | 100MB | Bootloader |

| Recovery | 546MB | Windows Recovery |

| C: | 187GB | Windows 10 Pro |

| Ubuntu | 50GB |

### Result
Dual boot confirmed. GRUB menu shows both Ubuntu and Windows OS options at startup.

<img width="900" height="1200" alt="IMG_5747" src="https://github.com/user-attachments/assets/6e7e5a52-52ec-40c6-b014-3bad38b25073" />

<img width="1400" height="740" alt="partition" src="https://github.com/user-attachments/assets/211db2f4-f346-4886-bd5c-23a26950df1e" />


### Notes
Never used PowerShell before until this. Extremely familiar with Ubuntu installer and GRUB now.

- - -

## Part 2: Encrypted USB Drive

### Goal
Encrypt a USB drive using VeraCrypt to store files

### Tools
- VeraCrypt

### Steps
1. Downloaded and installed VeraCrypt on Windows
2. Created encrypted volume on USB drive
3. Set strong passphrase and formatted the volume
4. Verified mount/unmount functionality

### Why I Picked VeraCrypt vs. BitLocker
- Cross-platform compatible mainly
- Open source and independently audited

- - -

## Skills
- Disk partitioning and managing volumes
- Dual-boot configuration and GRUB loader
- BitLocker troubleshooting
- USB drive encryption with VeraCrypt
- Physical hardware OS installation
