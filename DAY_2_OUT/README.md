# 🚀 Day 2 – Libraries, Synthesis Flows, and Flip-Flop Design  

Welcome back to the RTL workshop!  
On this second day, we move deeper into the design world and explore three essential pillars:  

1. The role of **timing libraries (.lib)** in digital design.  
2. A comparison between **hierarchical** and **flat synthesis** styles.  
3. Efficient **flip-flop modeling** in RTL for clean and synthesizable code.  

---

## 1️⃣ Timing Libraries  

### What is a PDK?  
A **Process Design Kit (PDK)** is a collection of files that describe a semiconductor process. The **SkyWater SKY130** PDK is open-source and provides:  
- Logic cells and device models  
- Timing and power data  
- Information about process corners and variations  

This lets EDA tools (like Yosys or OpenROAD) map RTL into real hardware.  

## PDK Overview
![Image](https://github.com/user-attachments/assets/03431828-8735-40bd-812a-b42dde90de86)
### Decoding `sky130_fd_sc_hd__tt_025C_1v80.lib`  
Breaking the name down:  
- **tt** → Typical process corner  
- **025C** → 25°C operating temperature  
- **1v80** → Supply voltage of 1.8 V  

Together, these parameters define the **environment in which timing and power values were measured**.  

### Exploring a `.lib` File  
The `.lib` file is plain text. You can open it with any editor:  

```bash
sudo apt install gedit
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
````

Inside, you’ll find **cell definitions, pin delays, setup/hold times, and power info** — everything the synthesis tool needs to map your design.

---

## 2️⃣ Synthesis Styles

### A. Hierarchical Synthesis

* **Approach:** Each RTL module is compiled independently; hierarchy remains intact.
* **Effect:** Easier to debug, since you can trace synthesized gates back to modules.

**Benefits:**

* Scales well for large designs
* Module-level reports are clearer
* Faster turnaround in early stages

**Drawbacks:**

* Optimizations are confined to individual modules
* Inter-module redundancy might remain

## Hierarchical Synthesis
![Image](https://github.com/user-attachments/assets/ad6470db-0dd7-455d-82ab-1cf2d0bf3967)
---

### B. Flat Synthesis

* **Approach:** The tool “flattens” the hierarchy into a **single-level netlist**.
* **Effect:** The tool has global visibility and can optimize aggressively.

**Benefits:**

* Eliminates redundant logic across modules
* Potentially higher performance

**Drawbacks:**

* Debugging is painful (no clear module boundaries)
* Runs longer on complex projects
* Output netlist may become bloated

## Flat Synthesis
![Image](https://github.com/user-attachments/assets/b680993e-f3b9-4a0e-9e05-c5cab0e1389a)
---

### C. Side-by-Side View

| Feature                | Hierarchical Flow     | Flat Flow              |
| ---------------------- | --------------------- | ---------------------- |
| **Structure**          | Modules preserved     | One big module         |
| **Optimization scope** | Local (per module)    | Global (entire design) |
| **Compile speed**      | Faster                | Slower for large RTL   |
| **Debugging**          | Straightforward       | Difficult              |
| **When to choose**     | Modular, reusable IPs | Performance-critical   |

---

## 3️⃣ Flip-Flop Modeling

Sequential circuits rely on flip-flops. Below are **clean, synthesizable Verilog templates** that synthesis tools handle efficiently.

### Asynchronous Reset D-FF

```verilog
module dff_async_reset (
    input  clk,
    input  rst_n,
    input  d,
    output reg q
);
  always @(posedge clk or negedge rst_n)
    if (!rst_n)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

* Reset overrides the clock
* Output clears immediately when `rst_n` goes low

---

### Asynchronous Set D-FF

```verilog
module dff_async_set (
    input  clk,
    input  set_n,
    input  d,
    output reg q
);
  always @(posedge clk or negedge set_n)
    if (!set_n)
      q <= 1'b1;
    else
      q <= d;
endmodule
```

* Forces `q = 1` immediately when set is active

---

### Synchronous Reset D-FF

```verilog
module dff_sync_reset (
    input  clk,
    input  rst,
    input  d,
    output reg q
);
  always @(posedge clk)
    if (rst)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

* Reset acts **only on clock edges**
* Common in designs needing predictable timing

---

## 4️⃣ Simulation & Synthesis Flow

### Simulating with Icarus Verilog

1. Compile your design + testbench:

   ```bash
   iverilog dff_async_reset.v tb_dff_async_reset.v
   ```
2. Run simulation:

   ```bash
   ./a.out
   ```
3. Open waveform in GTKWave:

   ```bash
   gtkwave tb_dff_async_reset.vcd
   ```

## Flip-Flop Waveform

![Image](https://github.com/user-attachments/assets/313cb9b0-8bcc-4bd6-adb7-fc6f59d49bb4)
---

### Synthesizing with Yosys

1. Launch yosys:

   ```bash
   yosys
   ```
2. Load the library:

   ```tcl
   read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
3. Load Verilog RTL:

   ```tcl
   read_verilog dff_async_reset.v
   ```
4. Run synthesis:

   ```tcl
   synth -top dff_async_reset
   ```
5. Map flip-flops:

   ```tcl
   dfflibmap -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
6. Perform technology mapping:

   ```tcl
   abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
7. Visualize netlist:

   ```tcl
   show
   ```

![Flip-Flop Schematic](images/flipflop_schematic.png)

---

## 📝 Takeaways

* `.lib` files provide the **timing and power foundation** for synthesis.
* **Hierarchical synthesis** = modular and fast, but limited optimizations.
* **Flat synthesis** = more optimized but harder to debug.
* Knowing how to **model flip-flops properly** ensures clean synthesis results.
* End-to-end flow: **Icarus Verilog → Yosys → Gate-level netlist visualization**.
