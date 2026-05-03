# Module 2: Timing Libs, Hierarchical vs Flat Synthesis & Efficient Flop Coding Styles

## 2.1 Introduction to .lib files

To open the library file:
```bash
gvim sky130_fd_sc_hd__tt_025C_1v80.lib
```
*(To remove the red highlight in `gvim`, do `Shift + :` and enter `syn off`.)*
![Image](../screenshots/Pasted%20image%2020260503183302.png)

### Understanding the Library Naming
File: `sky130_fd_sc_hd__tt_025C_1v80.lib`
- **sky130**: Name of the library (130nm technology).
- **tt**: Typical library corner (can be slow, typical, or fast).
- **025C**: Temperature (25°C).
- **1v80**: Voltage (1.80V).

### PVT Variations (Process, Voltage, Temperature)
- **Process**: Variation due to the fabrication process.

### Library Contents
The technology is CMOS and the delay model uses lookup tables. Units and operating conditions are specified first.
![Image](../screenshots/Pasted%20image%2020260503194327.png)
![Image](../screenshots/Pasted%20image%2020260503194432.png)

For every cell, we have different flavors.
![Image](../screenshots/Pasted%20image%2020260503194704.png)

Each cell definition contains different features:
- Leakage
- Area
- Power
- Port information
- Timing information

Example for an AND-OR-INVERT gate (`a21111o_1`):
![Image](../screenshots/Pasted%20image%2020260503195316.png)
Input and output characteristics.

## 2.2 Analyzing Cell Flavors

![Image](../screenshots/Pasted%20image%2020260503201510.png)
For example, in standard AND cells:
- **Area**: Highest in `and2_4`.
- **Power**: Highest in `and2_4`.
- **Delay**: Highest in `and2_0`.

## 2.3 Hierarchical vs Flat Synthesis

![Image](../screenshots/Pasted%20image%2020260503201927.png)
![Image](../screenshots/Pasted%20image%2020260503202059.png)
![Image](../screenshots/Pasted%20image%2020260503202326.png)
![Image](../screenshots/Pasted%20image%2020260503202419.png)

If there is an error in `show`, you can specify the module:
```tcl
show multiple_modules
```

![Image](../screenshots/Pasted%20image%2020260503202521.png)
The sub-modules show up as `u1` and `u2`. This represents **hierarchical synthesis**.

Write the hierarchical netlist:
```tcl
write_verilog -noattr multiple_modules_hier.v
```
View the file:
```bash
!gvim multiple_modules_hier.v
```
![Image](../screenshots/Pasted%20image%2020260503202846.png)

In the tutorial, the netlist was made by NAND and INV (x2) gates. This is due to:
![Image](../screenshots/Pasted%20image%2020260503203130.png)
PMOS transistors have lower mobility, which increases the required cell size. This is why sometimes specific mappings are chosen by the synthesizer.

## 2.4 Flat Synthesis

To generate a flat netlist (without preserving the module hierarchy):
```tcl
flatten
write_verilog -noattr multiple_modules_flat.v
```
![Image](../screenshots/Pasted%20image%2020260503203340.png)

Using the `flatten` command removes the sub-module boundaries.
![Image](../screenshots/Pasted%20image%2020260503203836.png)
![Image](../screenshots/Pasted%20image%2020260503203944.png)
Now `show` will display the design in flat mode, exposing all gates instead of sub-modules.

## 3. Various Flop Coding Styles

## 3.1 D Flip‑Flop with Asynchronous Reset

**Verilog Code:**
```verilog
module dff_asyncres (
    input clk,
    input d,
    input async_reset,
    output reg q
);
    always @(posedge clk, posedge async_reset) begin
        if (async_reset) q <= 1'b0;
        else q <= d;
    end
endmodule
```

**Synthesis Diagram:** ![Image](../screenshots/Pasted%20image%2020260503214104.png)

---

## 3.2 D Flip‑Flop with Asynchronous Set

**Verilog Code:**
```verilog
module dff_async_set (
    input clk,
    input d,
    input async_set,
    output reg  q
);
    always @(posedge clk, posedge async_set) begin
        if (async_set) q <= 1'b1;
        else q <= d;
    end
endmodule
```

**Synthesis Diagram:** ![Image](../screenshots/Pasted%20image%2020260503214011.png)

---

## 3.3 D Flip‑Flop with Synchronous Reset

**Verilog Code:**
```verilog
module dff_syncres (
    input clk,
    input d,
    input sync_rst,
    output reg q
);
    always @(posedge clk ) begin
        if (sync_reset) q <= 1'b0;
        else q <= d;
    end
endmodule
```

**Waveform:** reset has higher priority than input (`syncresdff_reshigh_priority`)

**Synthesis Diagram:** ![Image](../screenshots/Pasted%20image%2020260503213900.png)

---

## 3.4 Observations

- Asynchronous reset/set works independent of the clock.
- Synchronous reset depends on the clock edge.
- Different coding styles lead to different standard cells after synthesis.
- Choosing the proper coding style is important for timing and optimization.

---

## 3.5 Technology Mapping (DFF Mapping)

`dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80` – mapping library used during synthesis.

# 4. Optimisations

## 4.1 Theory – mul2 vs mult8

### mul2 Design

- Simple combinational circuit that multiplies an input by 2.
- Implemented as a left shift (`y = a << 1`).
- Very efficient – minimal logic, often just wiring.
- Synthesizes into simple gates or direct wiring.

### mult8 Design

- Slightly more complex circuit that multiplies an input by 8.
- Implemented as a left shift (`y = a << 3`).
- When written with extra modules or intermediate signals, synthesis tools can heavily optimise the logic.
- Demonstrates how redundant logic is removed during synthesis.

---

## 4.2 mul2 & mult8 Snapshots

- **mul2:** ![Image](../screenshots/Pasted%20image%2020260503213715.png)
- **mult8:** ![Image](../screenshots/Pasted%20image%2020260503213727.png)
