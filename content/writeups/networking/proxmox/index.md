---
title: "Why Proxmox Is One of the Best Hypervisors You Can Run in a Home Lab"
date: 2026-03-04
draft: false
tags: [proxmox, homelab, virtualization, open-source, self-hosted, infrastructure, zfs, truenas]
categories: [homelab, infrastructure]
---

# Why Proxmox Is One of the Best Hypervisors You Can Run in a Home Lab

There’s something incredibly satisfying about owning your infrastructure.

Not renting it.  
Not trusting it to some mysterious black box.  
**Actually owning it.**

That’s the spirit behind home labs, and it’s exactly why **Proxmox VE** has become one of the most powerful hypervisors available to builders, tinkerers, and serious self-hosters.

It’s fast.  
It’s powerful.  
It’s ridiculously flexible.

But most importantly…

**It’s open source.**

And that changes everything.

---

## Open Source Means No Secrets

One of the biggest problems with many enterprise virtualization platforms is that they are **closed systems**.

You don’t know what they’re doing behind the scenes.  
You don’t know what telemetry is being collected.  
And if something breaks, you’re often stuck waiting for a vendor patch.

Proxmox takes the opposite approach.

It is **fully open source**, built on:

- **Debian Linux**
- **KVM virtualization**
- **LXC containers**
- **ZFS storage**

That means:

- The code is visible
- The community is massive
- Development moves quickly
- You are never locked in

There are **no secrets**.

If something can be improved, someone in the community probably already has.

For people who believe in **owning their infrastructure**, that transparency matters.

---

## Enterprise Features Without Enterprise Pricing

One of the most surprising things about Proxmox is how many **enterprise-grade capabilities** come out of the box.

Right away you get:

- Virtual Machines (KVM)
- Containers (LXC)
- ZFS storage
- Software defined networking
- Snapshots
- Backups
- Web-based management
- API automation

But one feature stands above the rest.

### Clustering and High Availability

Proxmox servers can **cluster together**.

This allows multiple machines to operate as a **single virtualization platform**.

When clustered, you unlock features like:

- **Live migration of VMs**
- **Centralized management**
- **Distributed storage**
- **High availability**

High availability means that if one server goes down, **another node in the cluster can automatically restart the workload**.

For home labs, this is a massive capability that is usually reserved for expensive enterprise stacks.

I'll dive deeper into **running Proxmox in a true high-availability configuration in a future post**, because it's one of the most powerful things you can build at home.

---

## Proxmox Runs on Just About Any Hardware

Another reason Proxmox is so popular in the home lab world is simple:

**It runs on almost anything.**

Old desktops.  
Mini PCs.  
Enterprise servers.  
Custom builds.

If it can run Linux, there's a good chance **Proxmox will run beautifully on it**.

That flexibility makes it ideal for builders who want to scale infrastructure **without constantly replacing hardware**.

And if you're willing to experiment, Proxmox can power some incredibly capable systems.

---

## My Core Proxmox Host

At the center of my home infrastructure is a **custom-built Proxmox server** designed to handle virtualization, storage, and AI workloads.

Here’s what that machine looks like:

**CPU**

- AMD Ryzen 9 5900X  
- **12 cores / 24 threads**

This processor is a fantastic balance of performance and power efficiency. It gives plenty of cores for running multiple virtual machines and containers simultaneously.

**Memory**

- **128 GB RAM**

Virtualization loves memory. The more RAM you have, the more services you can comfortably run.

**PCIe Expansion**

This system has several PCIe cards installed to expand its capabilities.

One of them adds:

- **4 additional M.2 NVMe drives**

These are dedicated to running **VMs and containers**, giving extremely fast storage performance.

---

## Hosting My Own AI Server

One of the more exciting additions to the system is a **NVIDIA RTX 4070 GPU**.

This card allows the server to run:

- Local AI models
- Machine learning workloads
- Self-hosted AI tools

Running AI locally means:

- No API fees
- No external data exposure
- Full control over the models

And thanks to Proxmox, the GPU can be **passed directly to a virtual machine** that needs it.

---

## Storage: ZFS and TrueNAS Scale

Storage is where things get especially interesting.

My main storage pool consists of:

- **8 × 8TB SAS drives**
- Configured in **ZFS RAIDZ2**

This configuration provides:

- High redundancy
- Excellent data integrity
- Strong performance
- Protection against two drive failures

However, instead of letting Proxmox manage the ZFS pool directly, I take a slightly different approach.

### TrueNAS Scale Handles the Storage

All eight drives are **directly passed through to a TrueNAS Scale VM**.

Why?

Because **TrueNAS is simply better at managing ZFS**.

It provides:

- A far more advanced ZFS interface
- Dataset management
- Snapshots
- Replication
- Storage monitoring

Proxmox focuses on being the **hypervisor**.

TrueNAS focuses on being the **storage brain**.

That separation keeps things clean, powerful, and flexible.

---

## The Core Host… But Not the Only One

This server is the **core of my home lab**, but it’s not alone.

I run **several other Proxmox machines** throughout my network.

Some of them are:

- **Clustered together**
- Participating in shared workloads
- Capable of VM migration

Others are **standalone servers** dedicated to specific tasks.

This hybrid approach lets me balance:

- Reliability
- experimentation
- performance
- hardware availability

And the beauty of Proxmox is that it scales with you.

You can start with **one small machine**.

Then eventually grow into **a full cluster of servers**.

---

## Why Builders Love Proxmox

Proxmox hits a sweet spot that few platforms manage to achieve.

It is:

- **Open source**
- **Extremely powerful**
- **Flexible**
- **Hardware friendly**
- **Enterprise capable**

You’re not renting infrastructure.

You’re **building it**.

And once you start running your own virtualization platform, something interesting happens.

You stop thinking like a user.

You start thinking like an **infrastructure engineer**.

---

## The Beginning of a Bigger System

This core Proxmox host powers a large portion of my self-hosted environment, but it’s only the beginning.

In future posts, I’ll break down:

- **Running Proxmox in High Availability**
- **Building Proxmox clusters**
- **VM migration strategies**
- **Self-hosted AI infrastructure**
- **Designing a resilient home lab**

Because once you realize what tools like Proxmox can do…

You stop asking *what you should host.*

And start asking:

**“What else can I build?”**