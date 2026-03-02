---
title: "Building the Foundation: Where a Real Home Lab Actually Begins"
date: 2026-02-18
draft: false
tags:
  - homelab
  - networking
  - proxmox
  - pfsense
  - infrastructure
  - self-hosting
---

# Building the Foundation: Where a Real Home Lab Actually Begins

Key principle:

Terminate TLS at a single reverse proxy.  
Expose one entry point.  
Keep everything else internal.  

That’s discipline.

---

# The Real Reason to Do It This Way

This isn’t about running Plex on better hardware.

This is about understanding:

- How packets move
- How identity works
- How segmentation reduces risk
- How public DNS interacts with private services
- How virtualization abstracts hardware

You can buy ecosystems.

Or you can build infrastructure.

One teaches consumption.

The other teaches architecture.

---

# From Here

Once these three layers are stable:

1. Firewall
2. Hypervisor
3. Domain + DNS

Then you start layering services.

Then you experiment with containers.
Then you test identity providers.
Then you simulate production patterns.

But not before.

If I’m going to keep documenting this lab, the next logical step is hardening the firewall and wiring proper reverse proxy + certificate automation.

Because foundations first.

Always.

Alright. Let’s reset the narrative.

If your “home lab” started with Docker Compose and a YouTube tutorial titled *“10 Services You NEED to Self-Host”* — you skipped the hard part.

And the hard part is the part that actually matters.

A real home lab doesn’t begin with containers. It begins at the edge. It begins with control. It begins with infrastructure.

This is how I build mine. And if I’m going to do it right, I’m documenting it the same way I documented my Hugo journey — raw, practical, repeatable.

Three layers. No fluff.

---

# Step One: The Firewall — Your Real Starting Line

Your ISP combo modem/router is not your firewall.

It’s a convenience device.

If you want to learn how networks actually function, you need something that forces you to think about:

- Routing tables  
- NAT policies  
- VLAN segmentation  
- Stateful inspection  
- VPN termination  
- DNS behavior  

For me, that means open source.

## Picking the Platform

You’ve got solid options:

- **pfSense**
- **OPNsense**
- **OpenWrt**
- Even an old **SonicWALL** if that’s what’s sitting on your shelf

But if you can build it yourself on open hardware? Do it.

There’s something different about installing a firewall OS on bare metal and realizing:  
*This is my perimeter now.*

## Hardware Reality Check

You do not need rack gear.

You need:

- A CPU with AES-NI support (for VPN performance)
- Dual NICs (Intel preferred)
- 4–8GB RAM
- A small SSD

Old small form factor desktops are perfect.
A 4 port, 1G NIC will turn many PCs into a firewall.
Mini PCs with dual NICs are even better.
Repurpose first. Upgrade later.

## The One Rule: No Flat Networks

If everything is on 192.168.1.0/24, you’re not learning networking.

Minimum segmentation:

- VLAN 10 – Trusted devices  
- VLAN 20 – Servers  
- VLAN 30 – IoT  
- VLAN 40 – Guest  

Then write explicit firewall rules between them.

Block by default.
Allow intentionally.

That’s where the education starts.

---

# Step Two: The Hypervisor — Your Internal Cloud

Once the edge is solid, now you add compute.

This is where most people start. It shouldn’t be.

My choice here is simple:

**Proxmox VE.**

Not because it’s trendy. Because it’s practical.

## Why Proxmox?

- Open source
- KVM-based virtualization
- LXC containers
- ZFS built in
- Web UI that doesn’t fight you
- No artificial licensing walls

It gives you the same architectural patterns you’d see in enterprise environments, minus the procurement process.

And that’s the point.

## Hardware: Start Small, Think Long

You don’t need dual Xeons and a screaming rack.

Start with:

- 16–32GB RAM  
- 1 SSD or NVMe  
- Any modern CPU with virtualization extensions 
- 6th Gen Intel or newer for video encoding (Plex or Jellyfin) 

That’s enough to run:

- A reverse proxy
- DNS filtering
- Home Assistant
- Monitoring stack
- A Windows or Linux test VM
- A storage VM

You expand when your workloads justify it — not because Reddit told you to.

---

# Step Three: A Domain — The Piece Everyone Skips

This is the quiet difference between a hobby setup and infrastructure.

Register a domain.

Seriously.

It unlocks:

- Real TLS certificates via Let’s Encrypt
- Proper subdomain routing (app.yourdomain.com)
- Clean reverse proxy configuration
- Remote access without IP memorization
- Identity provider integrations later on

Without a domain, you’re stuck hacking around IP-based access.

With a domain, you’re building something structured.

## Static IP vs Dynamic DNS

If you can get a static IP, that’s ideal.

It simplifies inbound NAT.
It makes certificate validation predictable.
It removes variables.

If you can’t?

Dynamic DNS works fine.

Most firewalls support:

- Cloudflare API updates
- DuckDNS
- Native DDNS providers

Automation solves volatility.

---

# What This Looks Like in Practice

Your baseline topology should resemble something like:

Internet

↓

Firewall (pfSense / OpenWrt)

↓

Managed Switch (VLAN aware)

↓

Proxmox Host

├── Reverse Proxy (Nginx / Traefik)

├── DNS Filtering (Pi-hole / AdGuard)

├── Home Automation

├── Monitoring

└── Lab/Test Machines


Key principle:

Terminate TLS at a single reverse proxy.  
Expose one entry point.  
Keep everything else internal.  

That’s discipline.

---

# The Real Reason to Do It This Way

This isn’t about running Plex on better hardware.

This is about understanding:

- How packets move
- How identity works
- How segmentation reduces risk
- How public DNS interacts with private services
- How virtualization abstracts hardware

You can buy ecosystems.

Or you can build infrastructure.

One teaches consumption.

The other teaches architecture.

---

# From Here

Once these three layers are stable:

1. Firewall
2. Hypervisor
3. Domain + DNS

Then you start layering services.

Then you experiment with containers.
Then you test identity providers.
Then you simulate production patterns.

But not before.

If I’m going to keep documenting this lab, the next logical step is hardening the firewall and wiring proper reverse proxy + certificate automation.

Because foundations first.

Always.
