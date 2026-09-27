# Benchmark 003 — IRIS MK1 RTX 2060 SUPER Baseline

## Objective

Establish a repeatable local AI inference baseline for IRIS MK1 running on the NVIDIA GeForce RTX 2060 SUPER currently assigned to the IRIS virtual machine.

The purpose of this benchmark is to create a documented reference point for future Project BEYOND AI compute upgrades.

Future GPUs, compute nodes, and AI-focused hardware can be tested using the same model and methodology to provide direct before-and-after comparisons.

---

## Test Environment

| Component | Configuration |
|---|---|
| AI System | IRIS MK1 |
| Virtualization Host | Frankenstein |
| Hypervisor | Proxmox VE |
| Guest OS | Debian GNU/Linux 13 |
| GPU | NVIDIA GeForce RTX 2060 SUPER |
| GPU VRAM | 8 GB |
| NVIDIA Driver | 550.163.01 |
| CUDA Version | 12.4 |
| AI Runtime | Ollama |
| Model | iris-mk1:latest |
| Architecture | Llama |
| Parameters | 3.2B |
| Quantization | Q4_K_M |
| Context Length | 131,072 tokens |

IRIS MK1 uses a customized system prompt defining the IRIS identity and behavior.

No private IP addresses, host identifiers, or other environment-specific credentials are included in this benchmark.

---

## Methodology

Testing was performed directly inside the IRIS MK1 virtual machine.

Ollama's verbose output was used to capture:

- Model load duration
- Prompt evaluation performance
- Generation duration
- Generated token count
- Generation rate

GPU telemetry was collected using `nvidia-smi`.

Telemetry samples included:

- GPU utilization
- VRAM usage
- GPU temperature
- Reported GPU power draw

GPU telemetry was sampled every 250 milliseconds during the sustained inference test.

The same IRIS MK1 model was used throughout testing.

---

## Test 1 — Cold Start Inference

The first test was executed after the GPU was observed in an idle state.

### Pre-Test GPU State

| Metric | Result |
|---|---:|
| GPU Utilization | 0% |
| VRAM Usage | 1 MiB |
| GPU Temperature | 33°C |
| Reported GPU Power | ~8 W |

### Prompt

```text
Explain what Project BEYOND is in exactly 200 words.
```

### Results

| Metric | Result |
|---|---:|
| Total Duration | 3.891 s |
| Model Load Duration | 1.815 s |
| Prompt Tokens | 488 |
| Prompt Evaluation Duration | 156.741 ms |
| Prompt Evaluation Rate | 3,113.42 tokens/s |
| Generated Tokens | 255 |
| Generation Duration | 1.917 s |
| Generation Rate | 133.05 tokens/s |

The first run includes the cost of loading the model.

---

## Test 2 — Warm Inference

The identical prompt was executed again immediately after the first test while the model remained loaded.

### Results

| Metric | Result |
|---|---:|
| Total Duration | 2.013 s |
| Model Load Duration | 0.743 ms |
| Prompt Tokens | 488 |
| Prompt Evaluation Duration | 25.153 ms |
| Prompt Evaluation Rate | 19,401.26 tokens/s |
| Generated Tokens | 252 |
| Generation Duration | 1.984 s |
| Generation Rate | 126.99 tokens/s |

### Cold vs. Warm

Model load time decreased from approximately **1.815 seconds** to less than **1 millisecond** once IRIS was resident.

Total response time decreased from approximately **3.89 seconds** to **2.01 seconds**.

Generation performance remained in approximately the same range.

This demonstrates the difference between starting IRIS from an unloaded state and performing inference while the model is already resident.

---

## Test 3 — Sustained Inference

A longer generation was used to determine whether inference performance remained stable beyond a short response.

### Prompt

```text
Write a detailed technical explanation of virtualization, networking, storage, GPU passthrough, local AI inference, and monitoring in a modern homelab. Be comprehensive and continue for at least 1000 words.
```

### Ollama Results

| Metric | Result |
|---|---:|
| Total Duration | 12.107 s |
| Model Load Duration | 1.393 ms |
| Prompt Tokens | 516 |
| Prompt Evaluation Duration | 45.183 ms |
| Prompt Evaluation Rate | 11,420.22 tokens/s |
| Generated Tokens | 1,558 |
| Generation Duration | 12.057 s |
| Sustained Generation Rate | **129.22 tokens/s** |

The sustained generation rate remained consistent with the shorter inference tests.

---

## GPU Telemetry

During the sustained inference workload, GPU telemetry was recorded at approximately 250 ms intervals.

### Observed Results

| Metric | Result |
|---|---:|
| Peak GPU Utilization | **95%** |
| VRAM Usage | **2,554 MiB** |
| Peak GPU Temperature | **51°C** |
| Peak Reported GPU Power | **186.15 W** |

Before inference began, the loaded model remained resident in approximately **2,554 MiB of VRAM** while GPU utilization remained at 0%.

During generation, VRAM consumption remained effectively unchanged while GPU utilization increased into the 90% range.

This demonstrates that model residency and active inference represent substantially different GPU workload states.

> Power values in this benchmark are GPU telemetry reported by `nvidia-smi`. They should not be interpreted as total system or wall power consumption.

---

## Results Summary

| Test | Generation Rate |
|---|---:|
| Cold Start | 133.05 tokens/s |
| Warm Inference | 126.99 tokens/s |
| Sustained Inference | **129.22 tokens/s** |

The three tests place IRIS MK1 inference performance consistently around **~130 tokens per second** on the RTX 2060 SUPER.

The sustained workload is used as the primary baseline because it provides a longer observation window and simultaneous GPU telemetry.

### Primary BEYOND AI Baseline

**IRIS MK1 — RTX 2060 SUPER**

- Sustained generation: **129.22 tokens/s**
- Peak GPU utilization: **95%**
- VRAM usage: **2,554 MiB**
- Peak GPU temperature: **51°C**
- Peak reported GPU power: **186.15 W**

---

## Observations

### The RTX 2060 SUPER remains capable for IRIS MK1

Despite its age, the RTX 2060 SUPER provides responsive local inference for the current 3.2B Q4_K_M IRIS model.

### Current IRIS workloads are not VRAM constrained

The sustained test used approximately **2.5 GiB of the GPU's 8 GiB VRAM**.

This indicates that the current IRIS MK1 model does not approach the VRAM capacity of the RTX 2060 SUPER during this workload.

Future larger models may change this limitation significantly.

### Generation performance remained stable

Short inference tests produced approximately 127–133 tokens/s.

The longer 1,558-token generation sustained **129.22 tokens/s**, showing no obvious performance collapse during the measured workload.

### The GPU is actively utilized

GPU utilization reached **95%** during inference.

This confirms that the workload is making substantial use of the passed-through RTX 2060 SUPER rather than primarily executing on the VM's CPU resources.

---

## Known Limitations

This benchmark represents one model, one quantization level, one prompt workload, and one hardware configuration.

It does not measure:

- Larger language models
- Maximum context-length performance
- Concurrent inference
- Multiple simultaneous users
- CPU-only inference
- Alternative quantization levels
- AI training workloads
- Image generation
- Embedding workloads
- Retrieval-augmented generation performance
- Total system power consumption

The benchmark is intended as a reproducible baseline rather than a complete characterization of AI performance.

---

## Future Comparison

When Project BEYOND receives or installs new AI compute hardware, this benchmark should be repeated using:

- The same `iris-mk1:latest` model
- The same sustained inference prompt
- The same Ollama verbose metrics
- The same GPU telemetry sampling interval
- Equivalent software configuration where practical

Future results can then be compared against:

> **RTX 2060 SUPER baseline: 129.22 tokens/s sustained**

Potential future comparison platforms include:

- Newer consumer GPUs
- Dedicated AI compute nodes
- Higher-VRAM GPUs
- Multi-GPU configurations
- Mini PCs with integrated AI acceleration
- Workstation GPUs
- Enterprise GPU servers

---

## Conclusion

IRIS MK1 currently sustains approximately **129 tokens per second** using its 3.2B Q4_K_M model on an NVIDIA GeForce RTX 2060 SUPER.

During sustained inference, the GPU reached **95% utilization**, used approximately **2.5 GiB of VRAM**, peaked at **51°C**, and reported a maximum GPU power sample of **186.15 W**.

These results establish the first Project BEYOND local AI compute baseline.

Future AI hardware will not be evaluated by whether it simply *feels faster*.

It will have to beat a measured baseline.

> **129.22 tokens per second. That's the number to beat.**
