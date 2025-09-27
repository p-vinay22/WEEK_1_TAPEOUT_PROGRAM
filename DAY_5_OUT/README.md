#  Day 5 – Optimization in Synthesis

👋 Welcome to **Day 5** of the RTL workshop!
Today’s focus is on **Verilog synthesis optimization**, covering conditional logic (`if-else`), case handling, loops, generate blocks, and how **bad coding styles** can lead to **inferred latches** 😱.

We’ll also reinforce concepts through **hands-on labs** 🧪.

---

## 📑 Topics Covered

1️⃣ If-Else Statements in Verilog
2️⃣ Inferred Latches in Verilog
3️⃣ Labs on If-Else & Case Statements
4️⃣ For Loops in Verilog
5️⃣ Generate Blocks in Verilog
6️⃣ Ripple Carry Adder (RCA) 🔗
7️⃣ Labs on Loops & Generate Blocks
8️⃣ Summary

---

## 🔀 If-Else Statements in Verilog

✔️ **Usage:**
Conditional execution inside `always`, `initial`, tasks, or functions.

**Syntax:**

```verilog
if (condition) begin
    // Code when true
end else begin
    // Code when false
end
```

🔹 `else` is optional
🔹 Use `begin...end` for multiple statements

**Nested If-Else Example:**

```verilog
if (cond1) begin
    // Executes if cond1 true
end else if (cond2) begin
    // Executes if cond2 true
end else begin
    // Executes if none true
end
```

---

## ⚠️ Inferred Latches in Verilog

🟡 **Problem:**
Occurs when not all paths assign a value → synthesis tool infers a **latch**.

❌ Example (BAD):

```verilog
always @(a, b, sel) begin
    if (sel)
        y = a; // Missing 'else'
end
```

✅ Solution (GOOD):

```verilog
always @(a, b, sel) begin
    case(sel)
        1'b1 : y = a;
        default : y = 1'b0; // Covers all cases
    endcase
end
```

---

## 🧪 Labs on If-Else & Case Statements

🔹 **Lab 1 – Incomplete If Statement**

```verilog
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```

### 📸 Lab 1 
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/c213572d-f20b-4b13-9079-cd3826697d92" />
---

### 🔹  2 –  Synthesis Result of Lab 1
### 📸 Lab 2 Image 
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/0b9fbb7f-c681-4331-8418-5216e7c6ccbd" />
---

🔹 **Lab 3 – Nested If-Else**

```verilog
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```

### 📸 Lab 3 Image Here
<img width="1920" height="1080" alt="incomp_if2 waveform" src="https://github.com/user-attachments/assets/7427b418-e5c4-4fd5-a43d-1fac7c31fe40" />

---

### 🔹 Lab 4 – Synthesis Result of Lab 3
### 📸 Lab 4 Image Here
<img width="1920" height="1080" alt="incomp_if2 schematic" src="https://github.com/user-attachments/assets/2add5e58-2848-42e0-a433-4a79acd2c2fb" />

---

🔹 **Lab 5 – Complete Case Statement**

```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```

### 📸 Lab 5 Image Here
<img width="1920" height="1080" alt="comp_case_waveform" src="https://github.com/user-attachments/assets/7a4f8cd0-6c8b-4eb5-b57a-41107a80cc67" />

---

### 🔹 Lab 6 – Synthesis Result of Lab 5
### 📸 Lab 6 Image Here
<img width="1920" height="1080" alt="comp_case_schematic" src="https://github.com/user-attachments/assets/8bcbee87-38d0-4462-9267-855be4325031" />

---

🔹 **Lab 7 – Incomplete Case Handling**
⚠️ Wildcards (`?`) may cause **missing assignments**!

### 📸 Lab 7 Image Here
<img width="1920" height="1080" alt="incomp_case_waveform" src="https://github.com/user-attachments/assets/a382f40a-319f-4364-9800-57a0354ee2c7" />

---

### 🔹Lab 8 – Partial Assignments in Case

```verilog
case(sel)
    2'b00: begin
        y = i0;
        x = i2;
    end
    2'b01: y = i1; // x not updated!
    default: begin
        x = i1;
        y = i2;
    end
endcase
```

📸 Lab 8 Image Here

---

## 🔁 For Loops in Verilog

✔️ Used inside **procedural blocks** only
✔️ Synthesizable if iteration count fixed at compile-time

**Example – 4-to-1 MUX with For Loop**

```verilog
for (i = 0; i < 4; i = i + 1) begin
    if (i == sel)
        y = data[i];
end
```

---

## 🏗️ Generate Blocks in Verilog

✔️ Used for **repetitive hardware instantiation**
✔️ Works with `genvar`

**Example:**

```verilog
genvar i;
generate
    for (i = 0; i < 4; i = i + 1) begin : gen_loop
        and_gate u_and (.a(in[i]), .b(in[i+1]), .y(out[i]));
    end
endgenerate
```

---

## ➕ Ripple Carry Adder (RCA)

An **RCA** is built by chaining **Full Adders**:

* Each adder’s `carry-out` → next adder’s `carry-in`
* For n-bit addition → requires n full adders

📸 *RCA Diagram Here*

---

## 🧪 Labs on Loops & Generate Blocks

🔹 **Lab 9 – 4-to-1 MUX Using For Loop**
📸 *Lab 9 Image Here*
<img width="1920" height="1080" alt="mux_generate_waveform" src="https://github.com/user-attachments/assets/8ebdefcc-a38a-480f-8974-033a73ecbc3d" />

---

🔹 **Lab 10 – 8-to-1 Demux Using Case**
📸 *Lab 10 Image Here*
<img width="1920" height="1080" alt="demux_case_waveform" src="https://github.com/user-attachments/assets/45284233-9f2e-43b5-a55a-78f086a58838" />

---

🔹 **Lab 11 – 8-to-1 Demux Using For Loop**
📸 *Lab 11 Image Here*
<img width="1920" height="1080" alt="demux_generate_waveform" src="https://github.com/user-attachments/assets/a17216a6-6e29-4677-a144-ae87845c2fa3" />

---

🔹 **Lab 12 – 8-bit Ripple Carry Adder with Generate Block**
📸 *Lab 12 Image Here*
<img width="1920" height="1080" alt="rca_waveform" src="https://github.com/user-attachments/assets/216e39c1-289e-425b-aedb-006627e97db7" />

---

## 📊 Summary

✔️ Always use **complete if-else/case** to avoid unintended latches 🚫
✔️ `for` loops + `generate` → scalable & clean Verilog ✅
✔️ Assign **every signal in all paths** for combinational logic ⚡
✔️ Use **hands-on labs** to reinforce concepts 🧪

---
