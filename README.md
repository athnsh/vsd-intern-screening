# RTL Design and Synthesis – VLSI Internship Work

**Submitted by:** Athar  
**Date:** 2026-05-03  
**Program:** VSD FPGA Internship Screening

**Core Technologies:** Verilog | Iverilog | GTKWave | Yosys | Sky130 PDK

---

## About This Repository

This repository documents my hands-on experience and lab work completed during the VSD FPGA Internship screening process. The primary focus of this assessment is to understand the complete end-to-end digital design flow, transitioning from behavioural Verilog code (RTL) into a synthesized, gate-level netlist.

---

## Modules 

### [Module 1: Verilog RTL Design and Synthesis Basics](./Module1_Intro_to_Verilog_RTL/README.md)
This module establishes the foundational workflow for digital simulation and synthesis:
- **Functional Simulation:** Utilizing the open-source `iverilog` simulator to evaluate design adherence to specifications.
- **Waveform Analysis:** Using `GTKWave` to inspect `.vcd` files and visualize input/output signal changes.
- **Introduction to Synthesis:** Using `Yosys` to convert behavioural RTL into a mapped gate-level netlist.
- **Standard Cell Mapping:** Running synthesis using the `sky130_fd_sc_hd__tt_025C_1v80.lib` library to observe physical gate implementations.

### [Module 2: Timing Libraries and Optimization Techniques](./Module2_Timing_Libs_Synthesis/README.md)
This module dives deeper into standard cell details and how a synthesizer handles complex module hierarchies:
- **Understanding `.lib` files:** Exploring PVT (Process, Voltage, Temperature) variations and how standard cells are characterized.
- **Cell Flavor Analysis:** Comparing different versions of standard cells (e.g., `and2_0` vs `and2_4`) and their tradeoffs between speed, area, and power.
- **Hierarchical vs Flat Synthesis:** 
  - *Hierarchical:* Retaining sub-module boundaries (`u1`, `u2`) within the netlist.
  - *Flat:* Using the `flatten` command to dissolve boundaries and optimize across the entire design space.

---

## Tools and Technologies

| Tool | Application in Flow |
| :--- | :--- |
| **Verilog HDL** | The hardware description language used for both RTL modeling and testbench creation. |
| **Iverilog** | Open-source compiler and simulator used for functional verification of the RTL. |
| **GTKWave** | Graphical waveform viewer utilized to observe signal transitions from simulation dump files. |
| **Yosys** | The primary open-source framework used for RTL-to-gate-level logic synthesis. |
| **Sky130 PDK** | The 130nm open-source standard cell library used to physically map the synthesized logic. |

---

## What I Gained

- A practical, hands-on understanding of how the RTL to gate-level transformation occurs.
- The ability to confidently write stimulus testbenches and debug functional discrepancies using GTKWave.
- Insight into how synthesis tools interpret constraints and choose specific cell flavors to meet performance needs.
- A foundational understanding of static timing constraints (setup and hold times) and why varied cell delays are crucial for a functional chip.
