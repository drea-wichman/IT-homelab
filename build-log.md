# Build Log

Dated, reverse-chronological record of infrastructure projects.

---

## June 2-5, 2026

**Newsletters, console, and domain migration**

Built three automated newsletters: a daily cybersecurity briefing, a daily technical study guide, and a weekly GRC roundup. Wanted a private dashboard to receive them, so built a serverless console. Password protection required moving the domain to a new DNS provider, so migrated everything over and set up key-value storage for the dashboard. Set up a deployment pipeline so new content publishes automatically without touching the live site directly.

## March 28, 2026

**Custom domain email**

Set up DKIM, SPF, and DMARC records for a custom domain. Created forwarding addresses for public-facing and private use. Checked that all the DNS records were showing up correctly and everything passed.

## March 7-11, 2026

**IoT network isolation**

Put IoT devices on their own network so they can't reach my main devices. Separate SSID, separate credentials, completely isolated.

## March 8, 2026

**Disk encryption and dual-boot partitioning**

Full disk encryption on all personal devices. Set up a VeraCrypt encrypted USB drive. Documented the dual-boot partition layout (Windows 10 Pro / Ubuntu 24.04) on the home lab machine. Full setup in [Partitioning and Encryption](homelab/partitioning-and-encryption.md).

## March 6, 2026

**Browser compartmentalization**

Set up multiple Brave browser profiles to keep different contexts separate (personal, burner accounts, research). Each profile has its own cookies, history, and logins. No sync between profiles or devices.

## February 25, 2026

**Privacy stack architecture**

Documented the full privacy stack: encrypted DNS, VPN, tracker-blocking browser, end-to-end encrypted email and storage, hardware security keys, alias-based email on a custom domain. Each layer covers a different threat. Full setup in [Personal OpSec Setup](personal-opsec-setup.md).

## February 22, 2026

**Hardware 2FA hardening**

Got Nitrokey set up as primary two-factor authentication across critical accounts. Kept authenticator app as fallback for services that don't support hardware keys yet. Turned off weaker login options where I could.

## February 21, 2026

**Home lab: initial environment setup**

Installed VirtualBox 7.0 on the HP EliteDesk. Troubleshot a missing Visual C++ 2019 dependency and resolved it. Downloaded and installed Windows Server 2022, created the first VM. Installed with desktop experience, configured a static IP, installed Active Directory Domain Services, and promoted the server to a domain controller. Created the domain and first user account. Full setup in [Active Directory Setup](homelab/active-directory-setup.md).
