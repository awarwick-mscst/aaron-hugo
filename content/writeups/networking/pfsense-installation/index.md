---
title: "Installing pfSense CE on Bare Metal"
date: 2026-03-01

tags:
  - pfsense
  - firewall
  - networking
  - homelab

categories:
  - networking
---

# From Zero to Segmented: Installing pfSense CE the Right Way (With VLANs That Actually Teach You Networking)

Most home networks are flat.

One LAN. One WiFi. Everything trusts everything.

Your laptop can talk to your smart bulbs. Your TV can talk to your NAS. Your IoT camera can reach your workstation.

That’s not a lab. That’s a liability.

If you want a home lab that actually teaches real networking, segmentation, routing, firewall policy, and security boundaries, you need a real firewall.

Let’s build one with **pfSense CE**.

We’ll cover:

- Installing pfSense CE
- Installing and configuring the Package Manager
- Designing VLANs for real-world segmentation
- Best-practice firewall rules
- Secure baseline hardening
- Architecture decisions that scale with you

By the end, you won’t just have a firewall.

You’ll have infrastructure.

---

# What We’re Building

We’ll install **pfSense CE** as our perimeter firewall and router.

We’ll create these VLANs:

| VLAN | Purpose | Example Subnet |
|------|----------|----------------|
| VLAN 10 | Management | 10.10.10.0/24 |
| VLAN 20 | Secure / Workstations | 10.10.20.0/24 |
| VLAN 30 | Self-Hosted Servers | 10.10.30.0/24 |
| VLAN 40 | IoT | 10.10.40.0/24 |
| VLAN 50 | Guest WiFi | 10.10.50.0/24 |

Your managed switch will trunk these VLANs to pfSense.

This is how real networks are built.

---

# Step 1: Install pfSense CE

## 1 Download the ISO

Go to the official pfSense site and download:

- Architecture: AMD64
- Installer: USB Memstick
- Console: VGA
- Filesystem: ZFS (recommended)

Flash it to a USB drive using:

- `Rufus` (Windows)
- `balenaEtcher`
- `dd` on Linux

---

## 2️ Install on Your Firewall Hardware

Boot your firewall box and:

- Choose **Install**
- Use ZFS (mirror if you have two drives)
- Accept defaults unless you know why you’re changing them

After reboot, you’ll configure:

- WAN interface
- LAN interface

For now:
- WAN → your modem
- LAN → trunk port to your managed switch

---

# Step 2: Initial Web Configuration

From a device plugged into LAN:

- Visit: `https://192.168.1.1`
- Default login:
  - User: `admin`
  - Pass: `pfsense`

Run the setup wizard:
- Set hostname (e.g., `fw01`)
- Set domain (e.g., `lab.local`)
- Configure DNS (Cloudflare or Quad9 recommended)
- Change the admin password immediately

---

# Step 3: Update and Install Package Manager Tools

pfSense includes a package manager by default.

Go to:

**System → Update**

- Update to the latest pfSense CE version
- Reboot if required

Then go to:

**System → Package Manager**

Recommended packages:

- **System Patches** (apply security fixes fast)
- **pfBlockerNG-devel** (DNS/IP filtering)
- **Cron** (task scheduling)
- **ntopng** (traffic visibility)
- **OpenVPN Client Export** (if using VPN)

This turns pfSense from a router into a platform.

---

# Step 4: Create VLAN Architecture

Now the fun part.

Go to:

**Interfaces → Assignments → VLANs**

Create VLANs on your LAN physical interface:

| VLAN Tag | Description |
|----------|-------------|
| 10 | MGMT |
| 20 | SECURE |
| 30 | SELFHOST |
| 40 | IOT |
| 50 | GUEST |

Save.

Then:

**Interfaces → Assignments**

Add each VLAN as a new interface.

Enable them and assign:

| VLAN | Static IP |
|------|-----------|
| MGMT | 10.10.10.1/24 |
| SECURE | 10.10.20.1/24 |
| SELFHOST | 10.10.30.1/24 |
| IOT | 10.10.40.1/24 |
| GUEST | 10.10.50.1/24 |

Enable DHCP for each VLAN under:

**Services → DHCP Server**

---

# Switch Configuration (Critical)

On your managed switch:

- Port to pfSense → **Trunk**
  - Tagged: VLANs 10,20,30,40,50
- Access ports:
  - Workstation ports → VLAN 20
  - Server ports → VLAN 30
  - IoT devices → VLAN 40
  - AP trunk port → Tagged VLANs for SSIDs

On your wireless AP:
- SSID "Home" → VLAN 20
- SSID "IoT" → VLAN 40
- SSID "Guest" → VLAN 50

Now you’re thinking like a network engineer.

---

# Step 5: Firewall Rules (The Most Important Part)

By default, pfSense allows all outbound traffic from each VLAN.

We’re going to tighten that.

## Block Inter-VLAN by Default

For each VLAN:

1. Go to **Firewall → Rules → [VLAN]**
2. Create rule at top:

- Action: Block
- Source: VLAN subnet
- Destination: RFC1918 networks
- Description: "Block lateral movement"

---

## Allow Specific Traffic

### SECURE → SELFHOST

Allow:
- HTTPS (443)
- SSH (22 if needed)
- Any specific services

---

### IOT VLAN

Allow:
- DNS → pfSense only
- NTP → pfSense
- Internet access

Block:
- Access to other VLANs

---

### GUEST VLAN

Allow:
- Internet only
Block:
- All internal networks

---

### MGMT VLAN

Allow:
- Access to all VLANs
- pfSense Web GUI
- SSH

This VLAN should be wired only. No WiFi.

---

# Security Hardening Best Practices

## 1️ Disable Web GUI on WAN

System → Advanced → Admin Access  
Uncheck WAN access.

---

## 2️ Change Default Ports (Optional but Smart)

Move Web GUI from 443 to something custom.

---

## 3️ Enable DNS Resolver with DNSSEC

Services → DNS Resolver:
- Enable DNSSEC
- Disable forwarding mode (optional, advanced users)

---

## 4️ Enable pfBlockerNG

Block:
- Known malicious IP ranges
- GeoIP regions (optional)
- Ad/tracker DNS lists

This protects every VLAN automatically.

---

## 5️ Backup Configuration

Diagnostics → Backup & Restore  
Download your config after major changes.

Better: Store encrypted backup in your password manager.

---

# What You Just Built (And Why It Matters)

You now have:

- Proper VLAN segmentation
- Stateful firewall policy control
- IoT isolation
- Guest containment
- Real routing boundaries
- DNS-level filtering
- Infrastructure visibility

This is how enterprises structure networks.

And now it’s in your house.

---

# Want to Go Further?

Next-level upgrades:

- Install **Suricata** for IDS/IPS
- Create a DMZ VLAN
- Deploy HAProxy for reverse proxy
- Use WireGuard for remote access
- Run pfSense in HA pair

Your home lab is no longer a toy.

It’s a proving ground.

---

# Final Thought

Anyone can plug in a router.

Builders design networks.

When you segment your home lab with pfSense, you’re not just improving security,  you’re learning routing, policy enforcement, and infrastructure architecture the right way.

Flat networks are easy.

Engineered networks are powerful.

If you’re building one, you’re already ahead.