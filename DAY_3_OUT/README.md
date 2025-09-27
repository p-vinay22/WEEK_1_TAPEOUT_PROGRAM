#  Day 3: Combinational and Sequential Optimization

Welcome to **Day 3** of the workshop! 🎓 Today, we explore **optimization techniques** for combinational ⚡ and sequential ⏱️ circuits to improve efficiency and performance.

---

## 📑 Table of Contents

1. 🔢 [Constant Propagation](#constant-propagation)
2. 🔄 [State Optimization](#state-optimization)
3. 🧩 [Cloning](#cloning)
4. ⏳ [Retiming](#retiming)
5. 🧪 [Labs on Optimization](#labs-on-optimization)

   * Lab 1 - Lab 6

---

## 🔢 Constant Propagation

**📝 Definition:**
Constant propagation is a compiler optimization technique used in VLSI design to replace variables with their constant values during synthesis.

**⚙️ How it works:**

* 🔍 Analyzes design code for variables assigned constant values.
* ✏️ Replaces those variables directly in the logic.
* 🔗 Simplifies and reduces circuit size.

**✨ Benefits:**

* 📉 Reduced complexity → simpler & smaller logic
* ⚡ Faster operation → reduced delay
* 🏭 Resource optimization → fewer gates/FFs

---

## 🔄 State Optimization

**📝 Definition:**
Improves **FSMs** by reducing the number of states and optimizing state encoding and logic.

**🔧 Methods:**

* 🔀 **State Reduction:** Merge equivalent states
* 💻 **State Encoding:** Assign optimal binary codes
* ➗ **Logic Minimization:** Apply Boolean algebra / CAD tools
* 🔋 **Power Optimization:** Clock gating & low-power design

---

## 🧩 Cloning

**📝 Definition:**
Cloning duplicates logic cells or modules to **balance loads**, reduce wire lengths, and improve timing & power.

**📌 Procedure:**

1. 🔍 Identify critical paths
2. ➕ Duplicate the critical cell
3. 🔗 Redistribute connections
4. 📐 Place & route cloned cells
5. ✅ Verify improvements

---

## ⏳ Retiming

**📝 Definition:**
Retiming repositions registers 🕹️ in a circuit to optimize performance without altering functionality.

**⚙️ How it works:**

* Represent as directed graph 📊
* Move registers to balance delays
* Check constraints ⚖️
* Minimize clock period ⏱️ & power 🔋

---

## 🧪 Labs on Optimization

### 🧪 Lab 1: Simple Conditional Assignment

```verilog
module opt_check (input a, input b, output y);
	assign y = a ? b : 0;
endmodule
```

✨ Explanation:

* If `a = 1` → `y = b`
* Else → `y = 0`

🧹 Use `opt_clean -purge` to remove unused logic.

📸 **Lab 1 Output:**
![Image](https://github.com/user-attachments/assets/809f3256-cf68-40b8-b263-8373fd97835c)

---

### 🧪 Lab 2: Multiplexer Behavior

```verilog
module opt_check2 (input a, input b, output y);
	assign y = a ? 1 : b;
endmodule
```

✨ Explanation:

* If `a = 1` → `y = 1`
* Else → `y = b`

📸 **Lab 2 Output:**
![Image](https://github.com/user-attachments/assets/f382314e-248e-4f20-b8a3-8e91e7915401)

---

### 🧪 Lab 3: Exploration of Cloning

```verilog
module opt_check2 (input a, input b, output y);
	assign y = a ? 1 : b;
endmodule
```

📸 **Lab 3 Output:**
![Image](https://github.com/user-attachments/assets/53b933e3-2387-4744-bfb4-a827e79803c4)

---

### 🧪 Lab 4: Nested Ternary Simplification

```verilog
module opt_check4 (input a, input b, input c, output y);
	assign y = a ? (b ? (a & c) : c) : (!c);
endmodule
```

✨ Simplifies to → `y = a ? c : !c`

📸 **Lab 4 Output:**
![Image](https://github.com/user-attachments/assets/9dd053d9-90f8-4e37-99d6-32bbd92cd2e2)

---

### 🧪 Lab 5: D Flip-Flop with Asynchronous Reset

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk or posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

✨ Explanation:

* Reset → `q = 0`
* Else → `q = 1`

📸 **Lab 5 Output:**
![Image](https://github.com/user-attachments/assets/42bc25ed-2bf5-4bb4-8ce3-bffaf8f67f54)

---

### 🧪 Lab 6: Flip-Flop Always Setting Output

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk or posedge reset)
begin
	q <= 1'b1;
end
endmodule
```

✨ Explanation:

* `q` is **always 1** regardless of inputs.

📸 **Lab 6 Output:** <img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/25f14f63-6ca1-4f71-ac62-26419a6ea6fa" />

---

## 📊 Summary

| ⚙️ Technique            | 🎯 Purpose                                           |
| ----------------------- | ---------------------------------------------------- |
| 🔢 Constant Propagation | Simplify logic by replacing variables with constants |
| 🔄 State Optimization   | Reduce FSM states & optimize encoding/logic          |
| 🧩 Cloning              | Duplicate logic → timing & load balancing            |
| ⏳ Retiming              | Reposition registers → optimize timing & power       |

---

## 📝 Notes

* 🧹 Always use `opt_clean -purge` between `abc -liberty` and `synth -top` for best results.
* 🧪 Labs provide **hands-on** proof of optimization techniques.
