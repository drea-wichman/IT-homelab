# Personal Operational Security Setup

## Overview
When I started learning about cybersecurity formally, I realized that I was already thinking like a defender by being security conscious in my personal life. This documents my personal setup and transition away from Google ecosystem (Gmail, Chrome, and Google search) to privacy-focused alternatives (Proton, Tor, and DuckDuckGo).


## Email Strategy

### Domain
I have a main Proton email that I basically never use directly or give out.

- **Registration:** through Porkbun

- **Email Provider:** Proton Mail (end-to-end encrypted)

- **Authentication:** SPF, DKIM, DMARC all configured



### Email Aliasing (SimpleLogin)
Instead of giving services my real email, every website/company gets a separate and unique alias. All emails received by an alias are forwarded to my Proton email.

**Why I do this:**

- **Breach containment:** If(when) a service gets hacked, attackers only get that one alias. I disable it, create a new one, update my account. My other accounts stay safe.

- **Spam tracking:** If an alias starts getting spam, I know exactly which company leaked/sold my data and can disable that specific alias without touching anything else.

- **Replying to emails:** SimpleLogin utilizes "reverse alias" meaning I can reply to emails and it shows the alias as the sender instead of my real email.

## Password Management
Every account has a unique, randomly generated password (20+ characters). I don't know any of my passwords except my master password.

**Tool:** Proton Pass (zero-knowledge encryption)

**Why it matters:**
- Zero-knowledge protects "resting" emails/data upon arrival/creation, makes stolen data useless without master password
- End-to-end encryption protects emails/data in transit
- Proton itself cannot even see my data

## Multi-Factor Authentication

I use 2FA on every account that supports it.

**My hierarchy:**
1. Hardware security keys (Nitrokey) - for critical stuff like email and password manager
2. TOTP authenticator apps - for most accounts
3. SMS - only when there's no other choice

## Network Security
**Firewall:** Network + host also
**VPN:** Proton VPN running 24/7 on all devices
**IoT Network Isolation:** Separate network for IoT devices to isolate from primary devices

## Privacy Architecture

**How my traffic flows:**
```
┌──────────────┐
│   Device     │
│  (Brave/Tor) │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    Host      │
│   Firewall   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  Proton VPN  │
│ (encrypted   │
│  tunnel +    │
│  DNS)        │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Internet   │
│   (Sites)    │
└──────────────┘
```
**How it works together:**

All traffic is encrypted before leaving my device, VPN is my spokesperson handling all communication with the internet on my behalf, and router/ISP only see that I'm using a VPN but can't see the data because it's an encrypted tunnel.

**What each layer protects:**
* **Cloudflare DNS:** private domain lookups when VPN is off for whatever reason, no logging like Google 8.8.8.8 does
* **Proton VPN:** hides browsing from router/ISP, changes my location, no logs kept, handles DNS privately when active
* **Brave Browser:** blocks trackers and ads at browser level
* **Proton Mail:** end-to-end encrypted communications
* **Host Firewall:** blocks any uninvited incoming connections

## What I'm learning from this
- How authentication actually works in real life beyond theory
- What a breach response looks like in practice
- Threat modeling - what/who all I am actually protecting myself from
- Value of layered security - if one thing fails, I have backups

## What I Still Want to Add Later
- Another layer of security via home network
- Possibly another hardware security key for redundancy
- sort of curious about Cloudflare vs. Quad9 vs. NextDNS now
