# Day 4: Exploring Gate-Level Simulation, Verilog Assignments, and Synthesis-Simulation Consistency

Welcome to Day 4 of the RTL Workshop! This session dives into three fundamental aspects of digital design:

- Gate-Level Simulation (GLS)  
- Blocking versus Non-Blocking assignments in Verilog  
- Addressing Synthesis and Simulation discrepancies  

Both concepts and practical labs are covered to build a deep understanding.

---

## Overview

1. [Gate-Level Simulation (GLS)](#gate-level-simulation-gls)  
2. [Understanding Synthesis-Simulation Differences](#understanding-synthesis-simulation-differences)  
3. [Verilog Procedural Assignments](#verilog-procedural-assignments)  
   - 3.1 [Blocking Assignments](#blocking-assignments)  
   - 3.2 [Non-Blocking Assignments](#non-blocking-assignments)  
   - 3.3 [Key Differences](#key-differences)  
4. [Hands-On Labs](#hands-on-labs)  
5. [Key Takeaways](#key-takeaways)  

---

## 1.Gate-Level Simulation (GLS)

**What is GLS?**  
GLS simulates the synthesized gate-level netlist of a digital design to verify:

- Correct functionality after synthesis  
- Timing behavior including delays  
- Power and testability features such as scan chains  

**Purpose:**  
- Confirm synthesis accuracy (RTL to gates)  
- Detect timing violations using annotated delays  
- Validate test structures incorporated for manufacturing tests  

**When:**  
Performed immediately post-synthesis and before physical layout.

**Variants:**  
- **Functional GLS:** Logic-only simulation without timing delays  
- **Timing GLS:** Includes realistic timing information from delay files  

---

## 2.Understanding Synthesis-Simulation Differences

A mismatch between RTL simulation and gate-level simulation or hardware may arise due to:

- Use of unsupported or non-synthesizable constructs (e.g., `initial` blocks, delays)  
- Coding mistakes such as missing `else` statements or incomplete sensitivity lists  
- Differing interpretations of ambiguous RTL by simulation and synthesis tools  

**Best practice:** Maintain clean, fully synthesizable, and unambiguous RTL code to avoid discrepancies.

---

## 3.Verilog Procedural Assignments

Verilog provides two assignment types within procedural blocks:

### 3.1.Blocking Assignments (`=`)

- **Behavior:** Execute immediately in the order written  
- **Use case:** Ideal for combinational logic blocks (e.g., `always @(*)`)  
- **Example:**
```verilog
always @(*) y = a & b;
````

### 3.2.Non-Blocking Assignments (`<=`)

* **Behavior:** Schedule assignment to occur at the end of the timestep, allowing concurrent updates
* **Use case:** Used in sequential logic (e.g., triggered by clock edges)
* **Example:**

```verilog
always @(posedge clk) q <= d;
```

### Key Differences Summary

| Aspect            | Blocking (`=`)               | Non-Blocking (`<=`)               |
| ----------------- | ---------------------------- | --------------------------------- |
| Execution order   | Immediate, sequential        | Scheduled, concurrent             |
| Typical use       | Combinational logic          | Sequential (flip-flops)           |
| Update timing     | Right away, as code executes | After current simulation timestep |
| Hardware inferred | Combinational gates          | Sequential flip-flops             |

---

## 4.Hands-On Labs

### Lab 1: Multiplexer Using Ternary Operator

```verilog
module mux_ternary(input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

*Behavior:* Output equals `i1` if `sel` is 1; otherwise `i0`.

### lab1-output
![Image](https://github.com/user-attachments/assets/427b7927-23cf-4a10-aeb6-c42d986a472e)
---

### Lab 2: Synthesize the MUX Using Yosys

Follow the standard synthesis procedure with Yosys on the above module.

### lab2-output
![Image](https://github.com/user-attachments/assets/44d5c454-5ef8-47ae-a4ab-2112f26458d4)
---

### Lab 3: Gate-Level Simulation of Synthesized MUX

Perform GLS with this command (adjust file paths):

```bash
iverilog /path/to/primitives.v /path/to/sky130_fd_sc_hd.v mux_ternary.v testbench.v
```

### lab3-output
![Image](https://github.com/user-attachments/assets/ce1d03ed-130d-45df-86fa-4af9f4ae72e0)
---

### Lab 4: Illustrating Common MUX Pitfalls

```verilog
module faulty_mux(input i0, input i1, input sel, output reg y);
  always @(sel) begin
    if (sel)
      y <= i1;
    else
      y <= i0;
  end
endmodule
```

**Problems:**

* Sensitivity list incomplete — should include `i0`, `i1`, and `sel`
* Non-blocking assignments (`<=`) incorrectly used in combinational logic

**Fixed version:**

```verilog
always @(*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```

### lab4-output
![Image](https://github.com/user-attachments/assets/2ea0cfca-8b52-403c-b7bc-f883fe712137)
---

### Lab 5: GLS of the Faulty MUX

Run GLS on the faulty module; expect warnings or mismatches due to above errors.

### lab5-output
![Image](https://github.com/user-attachments/assets/f2550174-9800-4171-bb5a-e18e34be3e44)
---

### Lab 6: Demonstrating Blocking Assignment Order Issue

```verilog
module blocking_order_issue(input a, input b, input c, output reg d);
  reg x;
  always @(*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```

**Issue:** `d` uses old `x` value because `x` is updated after `d`.

**Correction:**

```verilog
always @(*) begin
  x = a | b;
  d = x & c;
end
```

### lab6-output
![Image](https://github.com/user-attachments/assets/7f9408c0-d216-43d7-8f1b-ee49d66ebc62)
---

### Lab 7: Synthesizing the Corrected Module

Synthesize the fixed module and analyze results.

### lab7-output
![Image](https://github.com/user-attachments/assets/2b4e163d-2318-4410-8e59-37ad6449b578)
---

## Key Takeaways

* GLS verifies design functionality, timing, and testability after synthesis.
* Avoid synthesis-simulation mismatches by writing clear and synthesizable RTL.
* Use blocking assignments for combinational logic and non-blocking for sequential.
* Practical labs illustrate typical pitfalls and best practices in RTL design.
