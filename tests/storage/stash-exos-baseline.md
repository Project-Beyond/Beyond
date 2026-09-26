# Benchmark 002 — Stash Sequential Storage Baseline

## Objective

Establish a repeatable sequential performance baseline for the current Project BEYOND bulk-storage system before Stash is migrated into dedicated storage infrastructure.

This benchmark is intended to answer a simple question:

> What can the current direct-attached Stash disk actually sustain before network, NAS, or storage-platform limitations are introduced?

---

## Test Environment

| Component | Configuration |
|---|---|
| Host | Frankenstein |
| Hypervisor | Proxmox VE 9.2 |
| Host OS | Debian GNU/Linux 13 |
| Storage Device | Seagate Exos 18 TB enterprise HDD |
| Filesystem | ext4 |
| Mount | `/mnt/pve/stash` |
| Benchmark Tool | fio 3.39 |
| Test File | 50 GiB |
| Block Size | 1 MiB |
| Queue Depth | 16 |
| I/O Engine | libaio |
| Direct I/O | Enabled |
| Jobs | 1 |

The benchmark used filesystem-level direct I/O against a temporary test file on Stash.

No destructive raw-disk testing was performed.

---

## Methodology

Initial testing used a 10 GiB test file with a 30-second time-based workload.

The sequential write result appeared consistent, but the initial sequential read produced unusually high transient throughput and an average result that did not appear representative of sustained physical disk performance.

Rather than publish the most favorable result, the methodology was changed.

A 50 GiB file was written and read exactly once using sequential 1 MiB I/O.

Before the final read test, filesystem buffers were synchronized and clean filesystem caches were dropped.

This larger single-pass workload was selected as the baseline because it produced stable, repeatable results without repeatedly cycling through the smaller test file.

---

## Sequential Write

### Result

| Metric | Result |
|---|---:|
| Data Written | 50.0 GiB |
| Throughput | **258 MiB/s** |
| Decimal Throughput | **270 MB/s** |
| IOPS | **257** |
| Average Latency | **62.08 ms** |
| Runtime | **198.7 seconds** |
| Disk Utilization | **96.96%** |
| Errors | **0** |

The disk sustained approximately **270 MB/s** across the complete 50 GiB write workload.

---

## Sequential Read

### Result

| Metric | Result |
|---|---:|
| Data Read | 50.0 GiB |
| Throughput | **264 MiB/s** |
| Decimal Throughput | **276 MB/s** |
| IOPS | **263** |
| Average Latency | **60.69 ms** |
| Runtime | **194.3 seconds** |
| Disk Utilization | **98.29%** |
| Errors | **0** |

The disk sustained approximately **276 MB/s** across the complete 50 GiB read workload.

---

## Results Summary

| Workload | Throughput |
|---|---:|
| Sequential Write | **258 MiB/s / 270 MB/s** |
| Sequential Read | **264 MiB/s / 276 MB/s** |

The read and write results are closely aligned and demonstrate consistent sequential performance from the current Stash disk.

---

## Initial Read Anomaly

An earlier 10 GiB time-based sequential read test reported approximately **362 MB/s** average throughput and included transient multi-gigabyte-per-second samples.

The test was repeated after synchronizing filesystem buffers and dropping clean filesystem caches, but the behavior remained.

Because those results did not appear representative of sustained physical disk performance, they were not selected as the baseline.

The benchmark was redesigned around a larger 50 GiB single-pass workload.

The resulting **276 MB/s** sequential read measurement was substantially more stable and is the value used for the Project BEYOND baseline.

---

## Analysis

Stash is currently a single direct-attached enterprise HDD inside Frankenstein.

Despite its simple architecture, the drive sustained approximately:

- **270 MB/s sequential write**
- **276 MB/s sequential read**

These results are particularly relevant to the BEYOND network roadmap.

A 1 GbE connection has a theoretical signaling rate of approximately **125 MB/s before protocol overhead**.

The current Stash disk therefore has enough sequential performance to exceed the practical capacity of a 1 GbE network connection.

This means moving Stash into dedicated network-attached storage without also increasing network bandwidth could introduce a new bottleneck.

---

## Relationship to Benchmark 001

Benchmark 001 established that Frankenstein's current 1 GbE network connection can sustain approximately:

- **946 Mbit/s inbound**
- **937 Mbit/s outbound**
- **946 Mbit/s aggregate with four parallel streams**

Benchmark 002 establishes that Stash can sustain approximately:

- **276 MB/s sequential read**
- **270 MB/s sequential write**

Together, the benchmarks demonstrate that the existing 1 GbE network is capable of operating near its expected limit while the underlying storage can exceed the throughput available through that link.

The next infrastructure improvement should therefore consider the network and storage architecture together rather than treating them as independent systems.

---

## Current Limitations

This benchmark represents the current configuration only.

Stash currently has:

- A single physical HDD
- No storage redundancy
- No dedicated NAS platform
- No independent storage host
- Direct attachment to Frankenstein

The benchmark measures sequential throughput and is not intended to characterize database, VM, random-I/O, or multi-user storage performance.

---

## Future Comparison

When Stash is migrated to dedicated storage infrastructure, this benchmark can be repeated to compare:

- Direct-attached vs network-attached storage
- 1 GbE vs 2.5 GbE
- 2.5 GbE vs 10 GbE
- Single-disk vs multi-disk storage
- Filesystem and storage-platform overhead
- Local vs network transfer performance

The current results provide the baseline against which those upgrades can be measured.

---

## Conclusion

The current Stash disk is capable of approximately **270–276 MB/s sustained sequential throughput**.

The storage device itself can therefore exceed the usable throughput of a 1 GbE network connection.

For Project BEYOND, the next storage evolution is not simply about adding capacity.

It is about building infrastructure capable of delivering the performance that the storage already has.

> **Measure first. Upgrade second.**
