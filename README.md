# FPGA Internship Screening – 12-Hour Lab Assessment

**Submitted by:** Athar  
**Date:** 2026-05-03  
**Program:** VSD FPGA Internship Screening

---

## Repository Structure

```text
.
├── Day1_Intro_to_Verilog_RTL/ # RTL design, simulation, and basic synthesis
│   ├── rtl/                   # Verilog source modules
│   ├── tb/                    # Testbench files
│   ├── waveforms/             # GTKWave screenshots (.png)
│   ├── sim_output/            # iverilog compiled outputs & .vcd files
│   └── README.md              # Day 1 Module Notes
│
├── Day2_Timing_Libs_Synthesis/ # Timing libs, hierarchical vs flat synthesis
│   ├── rtl/                   # RTL to be synthesized
│   ├── scripts/               # Yosys synthesis scripts (.ys)
│   ├── netlists/              # Generated gate-level netlists
│   ├── lib/                   # SKY130 standard cell library (.lib)
│   ├── reports/               # Synthesis reports & statistics
│   └── README.md              # Day 2 Module Notes
│
├── screenshots/               # Image assets used in documentation
└── resources/                 # Reference material, raw notes
```

---

## Modules Completed

### Day 1 – Introduction to Verilog RTL Design & Synthesis
- [x] Introduction to Open-Source Simulator (iverilog)
- [x] Working with iverilog and gtkwave
- [x] Introduction to Yosys and Logic Synthesis
- [x] Lab: Synthesizing RTL to Gate-Level Netlist

### Day 2 – Timing Libs, Hierarchical vs Flat Synthesis
- [x] Introduction to `.lib` files and PVT Variations
- [x] Analyzing Cell Flavors (Area, Power, Delay)
- [x] Hierarchical Synthesis
- [x] Flat Synthesis

---

## Tools Used

| Tool         | Purpose                            |
|--------------|------------------------------------|
| `iverilog`   | Verilog compilation & simulation   |
| `GTKWave`    | Waveform visualization             |
| `Yosys`      | RTL synthesis                      |
| `SKY130 PDK` | Standard cell library for synthesis|

