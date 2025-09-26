# Day 3: Combinational and Sequential Optimization

Welcome to **Day 3** of the workshop! Today, we explore optimization techniques for combinational and sequential circuits to improve efficiency and performance.

---

## Table of Contents

1. [Constant Propagation](#constant-propagation)  
2. [State Optimization](#state-optimization)  
3. [Cloning](#cloning)  
4. [Retiming](#retiming)  
5. [Labs on Optimization](#labs-on-optimization)  
   - Lab 1 - Lab 6
---

## Constant Propagation

**Definition:**  
Constant propagation is a compiler optimization technique used in VLSI design to replace variables with their constant values during synthesis. This simplifies the design and improves performance.

**How it works:**  
- Analyzes design code for variables assigned constant values.  
- Replaces those variables directly in the logic.  
- Allows simplification and reduction in circuit size.

**Benefits:**  
- Reduced complexity: simpler and smaller logic.  
- Performance improvement: faster operation and reduced delay.  
- Resource optimization: fewer gates or flip-flops required.  
---
## State Optimization

**Definition:**  
State optimization improves finite state machines (FSMs) by reducing the number of states and optimizing state encoding and logic to enhance efficiency.

**Methods:**  
- **State Reduction:** Merge equivalent states using algorithms.  
- **State Encoding:** Assign optimal binary codes to states.  
- **Logic Minimization:** Use Boolean algebra or CAD tools to minimize logic equations.  
- **Power Optimization:** Apply techniques such as clock gating to reduce dynamic power consumption.
---
## Cloning

**Definition:**  
Cloning duplicates logic cells or modules to optimize performance by balancing loads, reducing wire lengths, and improving timing and power characteristics.

**Procedure:**  
1. Identify critical paths using analysis tools.  
2. Duplicate the critical cell or module.  
3. Redistribute connections to balance load.  
4. Place and route the cloned cells.  
5. Verify improvements with timing and power analysis.
---
## Retiming

**Definition:**  
Retiming repositions registers (flip-flops) in a circuit to optimize performance without altering its functionality.

**How it works:**  
- Model the circuit as a directed graph.  
- Move registers to balance path delays.  
- Analyze constraints to maintain timing and functional equivalence.  
- Optimize register placement to minimize clock period and power consumption.
---
## Labs on Optimization

### Lab 1: Simple Conditional Assignment

```verilog
module opt_check (input a, input b, output y);
	assign y = a ? b : 0;
endmodule
````

**Explanation:**

* If `a` is true, `y` takes the value of `b`.
* Otherwise, `y` is `0`.

**Note:** Use `opt_clean -purge` in synthesis flow to remove unused logic.

**Image placeholder:**
### Lab 1 Output
![Image](https://github.com/user-attachments/assets/809f3256-cf68-40b8-b263-8373fd97835c)
---

### Lab 2: Multiplexer Behavior

```verilog
module opt_check2 (input a, input b, output y);
	assign y = a ? 1 : b;
endmodule
```

**Explanation:**

* When `a` is true, output `y` is `1`.
* Otherwise, `y` equals `b`.

**Image placeholder:**
### Lab 2 Output
![Image](https://github.com/user-attachments/assets/f382314e-248e-4f20-b8a3-8e91e7915401)
---

### Lab 3:Exploration of Cloning

```verilog
module opt_check2 (input a, input b, output y);
	assign y = a ? 1 : b;
endmodule
```

**Image placeholder:**
### Lab 3 Output
![Image](https://github.com/user-attachments/assets/53b933e3-2387-4744-bfb4-a827e79803c4)
---

### Lab 4: Nested Ternary Simplification

```verilog
module opt_check4 (input a, input b, input c, output y);
	assign y = a ? (b ? (a & c) : c) : (!c);
endmodule
```

**Explanation:**
Simplifies to: `y = a ? c : !c`

**Image placeholder:**
### Lab 4 Output
 ![Image](https://github.com/user-attachments/assets/9dd053d9-90f8-4e37-99d6-32bbd92cd2e2)
---

### Lab 5: D Flip-Flop with Asynchronous Reset

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

**Explanation:**

* On reset, output `q` is set to `0`.
* Otherwise, `q` is set to constant `1`.

**Image placeholder:**
### Lab 5 Output
![Image](https://github.com/user-attachments/assets/42bc25ed-2bf5-4bb4-8ce3-bffaf8f67f54)
---

### Lab 6: Flip-Flop Always Setting Output

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk or posedge reset)
begin
	q <= 1'b1;
end
endmodule
```

**Explanation:**
Output `q` is always set to `1` regardless of reset or clock signals.

**Image placeholder:**
![Lab 6 Output](./images/lab6_output.png)

---

## Summary

| Technique            | Purpose                                              |
| -------------------- | ---------------------------------------------------- |
| Constant Propagation | Simplify logic by replacing variables with constants |
| State Optimization   | Reduce FSM states and optimize encoding and logic    |
| Cloning              | Duplicate logic to improve timing and load balancing |
| Retiming             | Reposition registers to optimize timing and power    |

---

## Notes

* Use `opt_clean -purge` between `abc -liberty` and `synth -top` in your synthesis flow for cleaning optimization.
* Refer to the individual labs for hands-on examples demonstrating these optimization techniques.
