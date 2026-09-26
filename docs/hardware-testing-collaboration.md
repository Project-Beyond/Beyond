# Hardware Testing & Collaboration

Project BEYOND is an independent homelab project focused on building, testing, measuring, and documenting real infrastructure.

The project covers virtualization, networking, storage, local AI, GPU compute, automation, cybersecurity, and self-hosted services.

BEYOND is not built around synthetic reviews or short-term product showcases.

Hardware is installed into a working environment, tested against documented baselines, and evaluated as part of an evolving infrastructure platform.

---

## What BEYOND Tests

Project BEYOND is interested in hardware that can improve, expand, or solve documented limitations within the lab.

Current areas of interest include:

- 2.5 GbE and 10 GbE networking
- Network interface cards
- Managed and unmanaged switches
- SFP+ adapters, transceivers, and DACs
- Storage controllers and adapters
- SSD and NVMe storage
- NAS and storage hardware
- Memory
- Mini PCs and compute nodes
- Server and workstation hardware
- GPU compute hardware
- Rack infrastructure
- Cooling
- Power management
- UPS and PDU hardware
- Cables, adapters, and supporting infrastructure

Products are most useful to BEYOND when they can address a measurable limitation or enable a planned infrastructure upgrade.

---

## Testing Approach

Testing depends on the product, but may include:

### Installation

- Physical installation
- Hardware compatibility
- Driver and firmware setup
- Operating-system detection
- Proxmox compatibility
- Configuration process
- Installation issues and workarounds

### Performance

Where appropriate, testing may include:

- iperf3 network benchmarks
- fio storage benchmarks
- Before-and-after comparisons
- Sustained throughput
- Latency
- CPU and system utilization
- GPU workloads
- Virtual-machine workloads
- Multi-client or multi-stream testing

### Integration

Products may be evaluated inside the existing BEYOND environment rather than on an isolated test bench.

This can include integration with:

- Frankenstein — Proxmox virtualization host
- Stash — storage infrastructure
- IRIS — local AI platform
- Forge — development environment
- Hearth — hosted services environment
- The BEYOND 2.5 GbE network

---

## Baseline-Driven Testing

BEYOND attempts to establish performance baselines before replacing infrastructure.

This allows upgrades to be measured against the system they replace.

Examples include:

### Frankenstein Network Baseline

The current Frankenstein host operates through a 1 GbE connection.

Measured throughput:

| Test | Result |
|---|---:|
| Inbound | 946 Mbit/s |
| Outbound | 937 Mbit/s |
| Four-stream aggregate | 946 Mbit/s |

The results demonstrate that the current connection is operating near the practical limit of Gigabit Ethernet.

### Stash Storage Baseline

The current Stash storage disk is an 18 TB Seagate Exos enterprise HDD.

Measured sustained sequential throughput:

| Test | Result |
|---|---:|
| Sequential Write | 270 MB/s |
| Sequential Read | 276 MB/s |

The storage device can therefore exceed the throughput available through a 1 GbE network connection.

This creates a measurable use case for future multi-gigabit networking and dedicated storage infrastructure.

---

## What a Hardware Partner Receives

Depending on the hardware and project, testing may produce:

- Public installation documentation
- Hardware configuration notes
- Compatibility findings
- Benchmark methodology
- Benchmark results
- Before-and-after performance comparisons
- Architecture diagrams
- Build documentation
- Photographs of the deployment
- Long-term usage observations
- GitHub documentation
- Technical feedback

Results remain publicly accessible as part of the Project BEYOND documentation whenever appropriate.

---

## Independent Testing

Providing hardware does not guarantee a positive review, recommendation, or result.

Project BEYOND documents what happens during testing.

If a product works well, that will be documented.

If a product has limitations, compatibility problems, configuration difficulties, or unexpected behavior, those findings may also be documented.

The goal is useful technical information rather than promotional content.

---

## Disclosure

Hardware provided free of charge, discounted, loaned, sponsored, or otherwise supplied through a material relationship will be clearly disclosed in the relevant documentation.

Vendor involvement will not be hidden.

---

## Current Test Platform

The primary BEYOND test environment currently includes:

- Proxmox VE virtualization
- Debian and Windows virtual machines
- 2.5 GbE network infrastructure
- 1 GbE and 2.5 GbE endpoints
- Enterprise HDD storage
- NVMe VM storage
- NVIDIA GPU compute
- Local AI workloads
- Dedicated game/server workloads
- Development and automation environments

The environment intentionally includes both modern and repurposed hardware.

This allows upgrades to be evaluated against realistic systems rather than only against current-generation test benches.

---

## Current Opportunities

Several documented limitations provide immediate opportunities for hardware evaluation.

### Frankenstein Network Upgrade

Frankenstein currently connects to a 2.5 GbE network through a 1 GbE interface.

A compatible multi-gigabit NIC would allow direct before-and-after testing against the existing 946 Mbit/s baseline.

### Stash Network Evolution

Stash currently demonstrates approximately 270–276 MB/s sequential disk throughput.

Future dedicated storage infrastructure will require networking capable of carrying more of that performance than Gigabit Ethernet allows.

### Dedicated Storage

Stash currently consists of a single direct-attached enterprise HDD.

Future development includes independent NAS/storage infrastructure, redundancy, additional drives, and higher-speed networking.

### Dedicated AI Compute

IRIS currently uses an NVIDIA RTX 2060 SUPER passed through from Frankenstein.

Long-term development includes moving AI workloads toward dedicated GPU compute infrastructure.

---

## Collaboration Philosophy

Project BEYOND follows a simple process:

**Build it.  
Test it.  
Find the limitation.  
Document it.  
Improve it.  
Then repeat.**

Hardware collaborations should fit naturally into that process.

The objective is not to find products to advertise.

The objective is to find hardware worth testing against real problems.
