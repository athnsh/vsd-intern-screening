# Module 1: Introduction to Verilog RTL Design & Synthesis

## 1.1 Introduction to Open-Source Simulator (iverilog)

### Simulator
RTL Design is checked for adherence to the specification by simulating the design. The tool used here is the `iverilog` simulator.

- **Design:** The actual Verilog code which has the intended functionality to meet the required specifications.
- **Testbench:** The setup to apply stimulus (test vectors) to the design to check if it is obeying the specifications.

### How it works:
- The simulator looks for changes in the input signals.
- Upon a change in the input, the output is evaluated. If there is no input change, the output will not change.

![Image](../screenshots/Pasted%20image%2020260503150921.png)
![Image](../screenshots/Pasted%20image%2020260503151324.png)

## 1.2 Introduction to Labs

![Image](../screenshots/Pasted%20image%2020260503152042.png)
Learning about the folder structure.

### Working with iverilog and gtkwave
In the `verilog_files` folder, there is a testbench file for every Verilog file.
Run the following command:
```bash
iverilog good_mux.v tb_good_mux.v
```
This will generate an `a.out` file which will dump the `.vcd` file.
![Image](../screenshots/Pasted%20image%2020260503152745.png)
Click on a signal and use the arrow to go to transition.

### Looking at File Structure
Did not have `gvim` or `gedit` installed, so installing that.
![Image](../screenshots/Pasted%20image%2020260503153146.png)
![Image](../screenshots/Pasted%20image%2020260503153434.png)
*(This is a non-fatal Linux warning related to screen readers.)*

![Image](../screenshots/Pasted%20image%2020260503153531.png)
![Image](../screenshots/Pasted%20image%2020260503153629.png)
Stimulus generator after initializing inputs.

## 1.3 Introduction to Yosys

The synthesizer is the tool we use to convert RTL to a netlist. `yosys` is the tool used for this.
![Image](../screenshots/Pasted%20image%2020260503154216.png)
A netlist is the representation of the design in the form of cells in a library.

### Verification of Synthesis
![Image](../screenshots/Pasted%20image%2020260503154333.png)
The same testbench is used for verification.

## 1.4 Introduction to Logic Synthesis

RTL Design is the behavioral representation of the required specifications.
![Image](../screenshots/Pasted%20image%2020260503160008.png) 

Synthesis is the RTL-to-gate-level translator. The design is converted into gates and connections are made. The output is the netlist.
![Image](../screenshots/Pasted%20image%2020260503160141.png)

The `.lib` file is a collection of logical modules like AND, OR, NOT. It may contain different types of the same gate (e.g., SLOW, MEDIUM, and FAST versions of each n-input gate).
![Image](../screenshots/Pasted%20image%2020260503160339.png)

`T_CQ_A` = Propagation delay of A.
We need cells that make `T_COMBI` smaller.

### Why do we need slow cells?
![Image](../screenshots/Pasted%20image%2020260503163020.png)
- **Static Timing Analysis (STA):** We need to guarantee a minimum delay from A to B so that there are no hold delays.
- Data must arrive before the next clock edge and must not change immediately after the clock edge.
  - `T_CQ_A`: Delay from clock to the output of DFF_A.
  - `T_COMBI`: Delay of the combinational logic.
  - `T_HOLD_B`: Hold time requirement of DFF_B.
- Hence, we need fast cells to meet setup timing and slower cells for hold timing.

### Faster vs Slower Cells
- In digital logic, the load is capacitance.
- Faster charging and discharging of capacitance equals lesser cell delay.
![Image](../screenshots/Pasted%20image%2020260503163518.png)

### Selection of Cells
- We need to guide the synthesizer to select the flavor of cells that is optimal for the logic.
- More use of faster cells = bad in terms of power and area, may cause hold time violations.
- More use of slower cells = sluggish, may not meet performance needs.
- The guidance given to the synthesizer is in the form of **constraints**.
![Image](../screenshots/Pasted%20image%2020260503164754.png)

## 1.5 Lab: Introduction to Yosys

![Image](../screenshots/Pasted%20image%2020260503170254.png)
Invoking Yosys.

Read the liberty file:
```tcl
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Image](../screenshots/Pasted%20image%2020260503170819.png)

Read the Verilog file:
```tcl
read_verilog good_mux.v
```
![Image](../screenshots/Pasted%20image%2020260503170958.png)

Synthesize the top module:
```tcl
synth -top good_mux
```
![Image](../screenshots/Pasted%20image%2020260503171308.png)

Generate the netlist:
```tcl
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
*(This converts RTL to gates.)*

Show the design:
```tcl
show
```
![Image](../screenshots/Pasted%20image%2020260503171417.png)
![Image](../screenshots/Pasted%20image%2020260503171433.png)
*Note: This is happening because we changed the tool version on Codespace... and this is a standard observation in the industry.*

## 1.6 Writing Netlist

Now writing the netlist to a file:
```tcl
write_verilog good_mux_netlist.v
```

View the netlist:
```bash
!gvim good_mux_netlist.v
```
![Image](../screenshots/Pasted%20image%2020260503181706.png)

If many attributes are present, use `-noattr`:
```tcl
write_verilog -noattr good_mux_netlist.v
```
![Image](../screenshots/Pasted%20image%2020260503182008.png)
