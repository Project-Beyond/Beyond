# Project BEYOND — Hardware

Project BEYOND is built from a mixture of repurposed consumer hardware,
enterprise storage, dedicated networking equipment, and purpose-built upgrades.

This document represents the current hardware baseline of the lab and will
evolve as BEYOND expands.

> Hardware identifiers, serial numbers, MAC addresses, and other sensitive
> information are intentionally excluded.

---

# Frankenstein

### Primary Virtualization & Compute Host

Frankenstein is the current compute foundation of Project BEYOND.

Originally assembled from repurposed consumer hardware, Frankenstein now serves
as the primary Proxmox host and provides compute, storage, and GPU resources for
several of BEYOND's core environments.

## Hardware

| Component | Hardware |
|---|---|
| CPU | Intel Core i7-8700K |
| CPU Configuration | 6 cores / 12 threads |
| Motherboard | Gigabyte Z370 AORUS Gaming 7 |
| Memory | 64 GB DDR4-2133 |
| Primary GPU | NVIDIA GeForce RTX 2060 SUPER |
| Secondary GPU | NVIDIA GeForce GTX 1660 |
| Boot Storage | ~250 GB Samsung 850-series SATA SSD |
| VM Storage | 4 TB TEAMGROUP NVMe SSD |
| Bulk Storage | 18 TB Seagate Exos enterprise HDD |
| Network | 1 GbE uplink to 2.5 GbE BEYOND network |
| PSU | Corsair RM750 |
| Chassis | Repurposed Antec desktop chassis |
| Hypervisor | Proxmox VE 9.2 |
| Host OS | Debian GNU/Linux 13 |

---

## CPU & Memory

Frankenstein is powered by an Intel Core i7-8700K providing:

- 6 physical cores
- 12 threads
- 3.70 GHz base clock

The system contains 64 GB of DDR4 memory across four 16 GB DIMMs currently
operating at 2133 MT/s.

---

## GPU Compute

Frankenstein currently contains two NVIDIA GPUs.

### NVIDIA GeForce RTX 2060 SUPER

The RTX 2060 SUPER provides Frankenstein's primary GPU compute capability.

Its primary role within BEYOND is GPU-accelerated experimentation and local AI
workloads.

### NVIDIA GeForce GTX 1660

The GTX 1660 provides additional GPU resources for experimentation and future
workload separation.

The dual-GPU configuration gives BEYOND a platform for experimenting with GPU
allocation and workload distribution before AI services eventually migrate to
dedicated compute infrastructure.

---

## Storage

Frankenstein currently provides three primary storage tiers.

### System Storage

A Samsung SATA SSD provides storage for the Proxmox host operating system.

### High-Speed VM Storage

A 4 TB NVMe SSD provides high-speed storage for virtual machines and
performance-sensitive workloads.

### Bulk Storage

An 18 TB Seagate Exos enterprise hard drive currently provides high-capacity
storage for BEYOND.

As the environment grows, bulk storage is expected to migrate toward dedicated
storage infrastructure.

---

## Networking

Frankenstein currently connects to the BEYOND network using a 1 GbE interface.

The current BEYOND switching infrastructure supports 2.5 GbE, making
Frankenstein's 1 GbE interface an identified infrastructure bottleneck and
future upgrade opportunity.

Long-term network development is expected to include 10 GbE connectivity for
high-bandwidth compute and storage workloads.

---

## Current Role

Frankenstein currently provides infrastructure supporting:

- Proxmox virtualization
- IRIS
- Forge
- Hearth
- GPU experimentation
- Virtual machine storage
- Bulk storage
- Infrastructure testing

Frankenstein was intentionally built using repurposed hardware and continues to
serve as the foundation from which BEYOND is expanding.

---

## Planned Evolution

Frankenstein will remain part of Project BEYOND, but the long-term architecture
moves several responsibilities toward dedicated infrastructure.

Planned areas of expansion include:

- Dedicated AI compute
- Dedicated storage / NAS
- 10 GbE networking
- Dedicated application and service infrastructure
- Improved backup infrastructure
- Rack-mounted compute
- Power and environmental monitoring

The objective is not to replace Frankenstein.

**The objective is to let Frankenstein stop doing everything.**

---

# Additional BEYOND Hardware

Additional compute, networking, storage, and infrastructure hardware will be
documented here as the environment continues to grow.
