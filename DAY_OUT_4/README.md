# 🌟 Day 4: Exploring Gate-Level Simulation, Verilog Assignments & Synthesis-Simulation Consistency

Welcome to **Day 4 of the RTL Workshop!** 🚀  
This session dives into three fundamental aspects of digital design:

- ⚡ Gate-Level Simulation (GLS)  
- 🔀 Blocking vs Non-Blocking Assignments in Verilog  
- 🛠️ Handling Synthesis & Simulation Discrepancies  

Both **concepts** 🧠 and **practical labs** 🧪 are included to give you hands-on clarity.

---

## 📖 Overview

1. [⚡ Gate-Level Simulation (GLS)](#-gate-level-simulation-gls)  
2. [🔧 Synthesis-Simulation Differences](#-understanding-synthesis-simulation-differences)  
3. [🔀 Verilog Procedural Assignments](#-verilog-procedural-assignments)  
   - 3.1 [✏️ Blocking Assignments](#✏️-blocking-assignments)  
   - 3.2 [📌 Non-Blocking Assignments](#📌-non-blocking-assignments)  
   - 3.3 [📊 Key Differences](#-key-differences-summary)  
4. [🧪 Hands-On Labs](#-hands-on-labs)  
5. [✅ Key Takeaways](#-key-takeaways)  

---

## ⚡ Gate-Level Simulation (GLS)

**What is GLS?**  
GLS simulates the **synthesized gate-level netlist** 🏗️ to verify:

- ✅ Correct functionality after synthesis  
- ⏱️ Timing behavior including delays  
- 🔍 Power/testability features (e.g., scan chains)  

✨ **Purpose:**  
- 🎯 Ensure RTL → Gates conversion is correct  
- ⚠️ Detect timing violations (via delays)  
- 🧪 Validate test structures  

**When:** After synthesis 🔧, before layout 🖼️.  

**Variants:**  
- ⚙️ **Functional GLS** → Logic-only, no delays  
- ⏳ **Timing GLS** → Includes realistic delays  

---

## 🔧 Understanding Synthesis-Simulation Differences

Why do mismatches happen? 🤔  

- 🚫 Non-synthesizable constructs (`initial`, `#delay`)  
- ⚠️ Coding bugs (missing `else`, incomplete sensitivity lists)  
- 🔄 Ambiguity between simulation & synthesis tools  

💡 **Pro Tip:** Write **clean ✨, synthesizable ✅, and unambiguous 🔍 RTL**.

---

## 🔀 Verilog Procedural Assignments

Verilog supports **two styles** of assignments inside procedural blocks:

### ✏️ Blocking Assignments (`=`)

- 🕒 Execute **immediately**, sequentially  
- ⚡ Best for **combinational logic**  
- 💡 Example:
```verilog
always @(*) y = a & b;
````

---

### 📌 Non-Blocking Assignments (`<=`)

* ⏱️ Scheduled, concurrent updates
* 📍 Used in **sequential logic (flip-flops)**
* 💡 Example:

```verilog
always @(posedge clk) q <= d;
```

---

### 📊 Key Differences Summary

| Aspect                | ✏️ Blocking (`=`)     | 📌 Non-Blocking (`<=`)  |
| --------------------- | --------------------- | ----------------------- |
| ⏱️ Execution order    | Immediate, sequential | Scheduled, concurrent   |
| 🔧 Typical use        | Combinational logic   | Sequential (flip-flops) |
| ⏳ Update timing       | Instant               | End of timestep         |
| 🏗️ Hardware inferred | Combinational gates   | Sequential flip-flops   |

---

## 🧪 Hands-On Labs

### 🔹 Lab 1: Multiplexer Using Ternary Operator

```verilog
module mux_ternary(input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

📝 *Behavior:* `y = i1` if `sel=1`, else `i0`.

#### 🎨 Lab1 Output

![Image](https://github.com/user-attachments/assets/427b7927-23cf-4a10-aeb6-c42d986a472e)

---

### 🔹 Lab 2: Synthesize the MUX Using Yosys

🛠️ Run **Yosys** synthesis flow.

#### 🏗️ Lab2 Output

![Image](https://github.com/user-attachments/assets/44d5c454-5ef8-47ae-a4ab-2112f26458d4)

---

### 🔹 Lab 3: Gate-Level Simulation of Synthesized MUX

```bash
iverilog /path/to/primitives.v /path/to/sky130_fd_sc_hd.v mux_ternary.v testbench.v
```

#### ⚡ Lab3 Output

![Image](https://github.com/user-attachments/assets/ce1d03ed-130d-45df-86fa-4af9f4ae72e0)

---

### 🔹 Lab 4: Common MUX Pitfalls ⚠️

❌ Wrong version (non-blocking in comb. logic, bad sensitivity list):

```verilog
always @(sel) begin
  if (sel) y <= i1;
  else     y <= i0;
end
```

✅ Correct version:

```verilog
always @(*) begin
  if (sel) y = i1;
  else     y = i0;
end
```

#### 🐞 Lab4 Output

![Image](https://github.com/user-attachments/assets/2ea0cfca-8b52-403c-b7bc-f883fe712137)

---

### 🔹 Lab 5: GLS of the Faulty MUX

⚠️ Expect **warnings/mismatches** 🚨.

#### 🔥 Lab5 Output

![Image](https://github.com/user-attachments/assets/f2550174-9800-4171-bb5a-e18e34be3e44)

---

### 🔹 Lab 6: Blocking Assignment Order Issue 🔄

❌ Wrong order:

```verilog
d = x & c;
x = a | b;
```

✅ Fix:

```verilog
x = a | b;
d = x & c;
```

#### 🎭 Lab6 Output

![Image](https://github.com/user-attachments/assets/7f9408c0-d216-43d7-8f1b-ee49d66ebc62)

---

### 🔹 Lab 7: Synthesizing the Corrected Module

🛠️ Run synthesis → check gate netlist 🏗️.

#### 🌈 Lab7 Output

![Image](https://github.com/user-attachments/assets/2b4e163d-2318-4410-8e59-37ad6449b578)

---

## ✅ Key Takeaways

* ⚡ GLS = verifies post-synthesis correctness.
* 🧼 Keep RTL clean & synthesizable ✨.
* ✏️ Use **blocking** for combinational, 📌 **non-blocking** for sequential.
* 🧪 Labs show pitfalls 🚧 + best practices 🌟.

```

👉 Do you want me to also **add emoji banners** (like `--- 🌟 ---`) between sections for extra style, or keep it clean with just the headers?
```
