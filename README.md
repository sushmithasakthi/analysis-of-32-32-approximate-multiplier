# 32×32 Approximate Multiplier — FPGA Implementation

A hardware-efficient 32×32 signed multiplier using approximate computing techniques, implemented on the **Xilinx Artix-7 FPGA**. Designed for error-tolerant applications such as DSP, image processing, and machine learning accelerators.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Algorithm](#algorithm)
- [Simulation Results](#simulation-results)
- [Performance Analysis](#performance-analysis)
- [Comparison with Conventional Multipliers](#comparison-with-conventional-multipliers)
- [Applications](#applications)
- [Limitations & Future Work](#limitations--future-work)

---

## Overview

| Property | Detail |
|---|---|
| Operand Width | 32-bit signed |
| Output Width | 64-bit product |
| Algorithm | Booth's Multiplication |
| Target Platform | Xilinx Artix-7 FPGA |
| HDL | Verilog |
| Design Goal | Area & speed optimization via approximation |

The multiplier uses **OR gates in the lower significant bits** and **approximate adders in the higher significant bits**, significantly reducing logic complexity compared to conventional exact multipliers.

---

## Architecture

```
Inputs:   a [31:0]       — Multiplicand
          b [31:0]       — Multiplier
Output:   product [63:0] — 64-bit result
```

**Block Diagram:**

![Block Diagram](main/block_diagram.png)

The `multiplier_32x32` module implements a fully registered Booth multiplication pipeline in Verilog HDL with the following internal registers:

| Register | Role |
|---|---|
| `A` | Accumulator |
| `Q` | Multiplier register |
| `M` | Multiplicand register |
| `Qn_1` | Previous LSB (Booth look-ahead) |

---

## Algorithm

The design uses **Booth's Radix-2 Algorithm** over 32 iterative cycles:

1. Initialize `A`, `Q`, `M`, and `Qn_1`
2. Examine the pair `{Q[0], Qn_1}` at each cycle
3. Perform **add** or **subtract** based on Booth encoding:

| `Q[0]` | `Qn_1` | Operation |
|---|---|---|
| 0 | 0 | No operation |
| 0 | 1 | A = A + M |
| 1 | 0 | A = A − M |
| 1 | 1 | No operation |

4. Arithmetic right-shift `{A, Q}` after each cycle
5. Repeat for 32 cycles
6. Concatenate `{A, Q}` → 64-bit product

---

## Simulation Results

Functional simulation confirms correct output generation and stable operation under all tested input conditions.

**Simulation Waveform:**

![Simulation Waveform](results/waveform.png)

---

## Performance Analysis

### Power

| Parameter | Value |
|---|---|
| Total On-Chip Power | 82.429 W |
| Dynamic Power | 81.399 W |
| Static Power | 1.029 W |
| Junction Temperature | 125 °C |
| Thermal Margin | −95.2 °C |

> ⚠️ ~73% of total power is consumed by I/O resources. Junction temperature exceeds safe operating limits — thermal optimization is required.

![Power Report](results/power_analysis.png)

---

### Timing

| Parameter | Value |
|---|---|
| Total Delay | 33.969 ns |
| Logic Delay | 20.330 ns |
| Net Delay | 13.639 ns |

> Critical path delay is reduced compared to conventional multipliers, enabling higher clock frequencies.

![Timing Report](results/timing_report.png)

---

### Area (FPGA Resource Utilization)

| Resource | Utilization |
|---|---|
| LUT | 3% |
| I/O | 43% |

> Extremely low LUT utilization demonstrates the area savings achieved through approximation techniques.

![area](results/area.png)
![Utilization Report](results/utility.png)

---

## Comparison with Conventional Multipliers

| Metric | Conventional Multiplier | Approximate Multiplier |
|---|---|---|
| Area | High | **Low** |
| Delay | Higher | **Lower** |
| Speed | Moderate | **High** |
| Accuracy | Exact | Approximate |
| Power Efficiency | Moderate | Needs Optimization |

---

## Applications

This design is suitable for any system where **speed and area efficiency** outweigh the need for exact arithmetic:

- Digital Signal Processing (DSP)
- Image & Video Processing
- Video Compression
- Multimedia Systems
- Machine Learning / AI Accelerators
- Error-Tolerant Computing

---

## Limitations & Future Work

**Current limitations:**
- High dynamic power consumption
- Junction temperature exceeds safe limits (125 °C)
- Inherent accuracy trade-off due to approximation

**Planned improvements:**
- Thermal-aware floorplanning and I/O optimization
- Exploration of higher radix Booth encoding (Radix-4)
- Error characterization across representative DSP workloads
- Power gating and clock gating strategies

---

## Repository Structure

```
.
├── main/
│   └── Block_Diagram.png
├── results/
│   ├── waveform.png
│   ├── power_report.png
│   ├── timing_report.png
│   └── utility.png
├── src/
│   └── multiplier_32x32.v
└── README.md
```

---

> *This project is intended for educational and research purposes.*
