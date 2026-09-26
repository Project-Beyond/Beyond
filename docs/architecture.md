# Project BEYOND — Architecture

Project BEYOND is a modular homelab environment designed around virtualization,
local AI, storage, networking, development, automation, and self-hosted services.

The architecture is intentionally designed to evolve as new capabilities and
hardware are added.

> This documentation represents the logical architecture of BEYOND.
> Sensitive network information, credentials, addressing, and security
> configuration are intentionally excluded.

---

## Current Architecture

```mermaid
flowchart TD

    INTERNET["Internet<br/>2 Gbps Fiber"]

    ROUTER["Gateway / Router"]

    CORE["BEYOND Network<br/>2.5 GbE"]

    MAIN["Primary Workstation<br/>Administration & Development"]

    FRANK["FRANK<br/>Proxmox Compute Host"]

    IRIS["IRIS<br/>Local AI Assistant"]

    FORGE["FORGE<br/>Development Environment"]

    HEARTH["HEARTH<br/>Services & Game Servers"]

    STASH["STASH<br/>Storage & Backup"]

    INTERNET --> ROUTER
    ROUTER --> CORE

    CORE --> MAIN
    CORE --> FRANK
    CORE --> STASH

    FRANK --> IRIS
    FRANK --> FORGE
    FRANK --> HEARTH
