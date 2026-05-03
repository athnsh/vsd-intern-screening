# Introduction to Verilog RTL Design & Synthesis

## Introduction to open-source simulator - iverilog L1

### Simulator
RTL Design is checked for adherence to the spec by simulating the design, the tool used here is iverilog simulator.

Design is the actual verilog codes which has the intended functionality to meet the required specs

Testbench is the setup to apply stimulus (test_vectors) to the design to check if obeying spec

How it works:
- simulator looks for changes in input signal
- upon change to input output is eval, if theres no input change output wont change

![[Pasted image 20260503150921.png]]
![[Pasted image 20260503151324.png]]

## Introduction to lab L2
![[Pasted image 20260503152042.png]]
learning about the folder struct

## Introduction to Lab L3
how to work with iverilog and gtkwave, verilog_files folder: there is a tb file for every v file
run `iverilog good_mux.v tb_good_mux.v` this will generate an a.out file which will dump the vcd file
![[Pasted image 20260503152745.png]]
click on a signal and use the arrow to go to transition

## Introduction to Lab L4
looking at file structure, did not have gvim/gedit installed so installing that
![[Pasted image 20260503153146.png]]
![[Pasted image 20260503153434.png]]
this is a linux warning related to screen readers and all, non fatal

![[Pasted image 20260503153531.png]]
![[Pasted image 20260503153629.png]]
stimulus generator after initialize inputs

## Introduction to yosys L5
Synthesizer is the tool we use to convert RTL to netlist, yosys is the tool used 
![[Pasted image 20260503154216.png]]
netlist is the representation of design in the form of cells in lib
### Verification of Synthesis
![[Pasted image 20260503154333.png]]
same testbench is used

## Introduction to logic synthesis L6
RTL Design is the behavioral representation of the required specs 
![[Pasted image 20260503160008.png]] 
Synthesis is the RTL to gate level translator, design is converted into gates and connection is made, the output is the netlist
![[Pasted image 20260503160141.png]]
.lib is the collection of logical modules like AND OR NOT, it may contain different type sof the same gate, may contain SLOW, MEDIUM and FAST versions of each of those n-input gates
![[Pasted image 20260503160339.png]]
T_CQ_A = propagation delay of A
So we need cells that make T_COMBI Smaller
### Why we need slow cells?
![[Pasted image 20260503163020.png]]
- Static Timing Analysis 
- we need to guarantee a min delay from A to B so that there are no hold delays 
- data must arrive before next clk edge and must not change immediately after clk edge
	- `T_CQ_A` → delay from clock → output of DFF_A
	- `T_COMBI` → delay of combinational logic
	- `T_HOLD_B` → hold time requirement of DFF_B
- hence we need cells that work fast for setup timing and slower cells for hold timing

### Faster vs Slower cells
- Load in Digital logic is capacitance
- faster the charging and discharging of C == lesser cell delay![[Pasted image 20260503163518.png]]

### Selection of cells
- Need to guide synthesizer to select flavour of cells that is optimal for logic
- more use of faster = bad in terms of power and area, hold time violations maybe
- more use of slower = sluggish, may not meet performance need
- guidance given to synthesizer = constraints
![[Pasted image 20260503164754.png]]

## Lab Introduction to yosys L7
![[Pasted image 20260503170254.png]]
invoking yosys
`read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
![[Pasted image 20260503170819.png]]
`read_verilog good_mux.v`
![[Pasted image 20260503170958.png]]
`synth -top good_mux`
![[Pasted image 20260503171308.png]]
now generate netlist: `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
convert rtl to gate 

`show`
![[Pasted image 20260503171417.png]]
![[Pasted image 20260503171433.png]]
actual, this is happening because we changed the tool version on codespace…and this is a standard observation in industry

## Lab L9
now writing netlist

`write_verilog good_mux_netlist.v`
`!gvim good_mux_netlist.v`
![[Pasted image 20260503181706.png]]
if many attributes are present: `write_verilog -noattr good_mux_netlist.v`
![[Pasted image 20260503182008.png]]

# Timing libs, hierarchical vs flat synthesis and efficient flop coding styles
## Introduction to .lib L10
`gvim sky130_fd_sc_hd__tt_025C_1v80.lib`
to remove the red highlight we do shift+colon syn off
![[Pasted image 20260503183302.png]]

sky130_fd_sc_hd__tt_025C_1v80.lib
sky130: name of library 130nm
tt:          typical library (slow, typical, fast)
024C:     temperature

PVT: PROCESS VOLTAGE TEMPERATURE (variations)
	process: variation due to fabrication

technologies is cmos tech and delay model is lookup table, then units are mentioned then operating conditions ![[Pasted image 20260503194327.png]]
![[Pasted image 20260503194432.png]]
for every cell we have different flavours
![[Pasted image 20260503194704.png]] 
then it contains different features: leakage, area number, power port info, timing info
AND OR gate: a21111o_1
![[Pasted image 20260503195316.png]]
input and output

## L12
![[Pasted image 20260503201510.png]]area number highest in and2_4
power highest in and2_4
delay highest in and 2_0

## Hierarchical vs Flat Synthesis L13
![[Pasted image 20260503201927.png]]
![[Pasted image 20260503202059.png]]
![[Pasted image 20260503202326.png]]
![[Pasted image 20260503202419.png]]
error in show
so we use `show multiple_modules`

![[Pasted image 20260503202521.png]]
shows up as u1 and u2, hierarchical

`write_verilog -noattr multiple_modules_hier.v`
`!gvim multiple_modules_hier.v`
![[Pasted image 20260503202846.png]]
in the tutorial netlist was made by nand and inv x2
which is due to 
![[Pasted image 20260503203130.png]]
pmos has low mobility and the cell size also increases thats why its bad

## Lab14
flatten 
`write_verilog -noattr multiple_modules_flat.v`
![[Pasted image 20260503203340.png]]
`flatten` command
![[Pasted image 20260503203836.png]]
![[Pasted image 20260503203944.png]]
show in flatten mode