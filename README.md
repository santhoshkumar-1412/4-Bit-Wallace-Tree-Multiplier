
# 4-Bit Wallace Tree Multiplier | Verilog HDL

<p align="center">
  <b>RTL Design | Digital Arithmetic | Functional Verification</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Language-Verilog-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Simulation-Synopsys%20VCS-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Debugging-Synopsys%20Verdi-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Waveform-FSDB-purple?style=for-the-badge" />
</p>

## 1. Project Overview

This project presents the RTL design and functional verification of a **4-bit × 4-bit unsigned Wallace Tree Multiplier** using Verilog Hardware Description Language (HDL).

The Wallace Tree Multiplier is a high-speed multiplication architecture that reduces partial products using Half Adders (HA) and Full Adders (FA) in successive reduction stages.

The design accepts two 4-bit unsigned binary operands and generates an 8-bit multiplication result.

The RTL implementation is simulated using **Synopsys VCS**, while waveform visualization and signal-level debugging are performed using **Synopsys Verdi**.

## 2. Design Specifications

| Parameter | Description |
|---|---|
| Design Name | 4-Bit Wallace Tree Multiplier |
| Design Type | Combinational Logic |
| Architecture | Wallace Tree Multiplier |
| Input Operand A | 4-bit unsigned |
| Input Operand B | 4-bit unsigned |
| Output Product | 8-bit |
| Arithmetic Operation | Binary Multiplication |
| Basic Components | Half Adder and Full Adder |
| HDL | Verilog |
| RTL Simulator | Synopsys VCS |
| Waveform Viewer | Synopsys Verdi |
| Waveform Format | FSDB |

## 3. Design Methodology

The Wallace Tree Multiplier performs multiplication through partial product generation and successive reduction stages.

### 3.1 Partial Product Generation

Four partial product rows are generated using bitwise AND operations between the multiplicand and individual bits of the multiplier.

The partial product equation is:

`PP_i = A & {4{B_i}}`

Where:

- `A` represents the 4-bit multiplicand.
- `B_i` represents an individual multiplier bit.
- `PP_i` represents the corresponding partial product row.

The partial products are shifted according to their corresponding multiplier bit positions.

### 3.2 Wallace Tree Reduction

The generated partial products are reduced using Half Adders and Full Adders.

- Full Adders reduce three input bits of the same weight into a sum bit and a carry bit.
- Half Adders reduce two input bits into a sum bit and a carry bit.
- Sum bits remain in their respective weight columns.
- Carry bits are propagated to the next higher weight column.

The reduction process continues until the partial products are reduced to the final product representation.

### 3.3 Product Generation

The final sum and carry outputs are combined to produce the 8-bit multiplication result.

**Multiplication Operation:**

`P = A × B`

For the maximum 4-bit unsigned input values:

`P_max = 15 × 15 = 225`

**Binary Representation:**

`(1111)₂ × (1111)₂ = (11100001)₂`

**Final 8-bit Product:**

`P[7:0] = 11100001₂ = 225₁₀`

Therefore, an 8-bit output is sufficient to represent the complete unsigned multiplication result.


## 4. RTL Module Architecture

The project consists of the following Verilog modules:

| Module | Functionality |
|---|---|
| `ha` | Implements Half Adder logic |
| `fa` | Implements Full Adder logic |
| `wallace_tree_multiplier` | Implements partial product generation and Wallace Tree reduction |
| `wallace_tree_multiplier_tb` | Testbench for functional verification |

### 4.1 Half Adder (HA)

The Half Adder performs the addition of two single-bit inputs.

**Boolean Expressions:**

\[
S = A \oplus B
\]

\[
C = A \cdot B
\]

### 4.2 Full Adder (FA)

The Full Adder performs the addition of three single-bit inputs and generates Sum and Carry outputs.

**Boolean Expressions:**

\[
S = A \oplus B \oplus C_{in}
\]

\[
C_{out} = AB + BC_{in} + AC_{in}
\]

## 5. Repository Structure

```text
4-bit-wallace-tree-multiplier/
│
├── wallace_tree_multiplier.v
│   └── RTL source code (HA, FA and multiplier)
│
├── wallace_tree_multiplier_tb.v
│   └── Functional verification testbench
│
├── screenshots/
│   └── verdi_waveform.png
│
├── README.md
│
└── .gitignore
```

Generated simulation files such as FSDB databases, compiled executables and intermediate simulation directories can be maintained locally rather than committed to GitHub.

## 6. Functional Verification

A Verilog testbench is developed to verify the functional behavior of the Wallace Tree Multiplier.

The testbench applies five different combinations of input operands at 10 ns intervals and observes the corresponding 8-bit multiplication results.

### 6.1 Test Scenarios

| Test Case | Operand A | Operand B | Expected Product |
|:---:|:---:|:---:|:---:|
| TC1 | 0010 (2) | 0011 (3) | 00000110 (6) |
| TC2 | 0100 (4) | 0101 (5) | 00010100 (20) |
| TC3 | 0110 (6) | 0011 (3) | 00010010 (18) |
| TC4 | 1000 (8) | 0010 (2) | 00010000 (16) |
| TC5 | 1111 (15) | 1111 (15) | 11100001 (225) |

### 6.2 Expected Simulation Output

```text
Time=0  A=0010 B=0011 Product=00000110
Time=10 A=0100 B=0101 Product=00010100
Time=20 A=0110 B=0011 Product=00010010
Time=30 A=1000 B=0010 Product=00010000
Time=40 A=1111 B=1111 Product=11100001
```

## 7. Simulation Environment

The design is simulated using Synopsys VCS, and waveform analysis is performed using Synopsys Verdi.

### 7.1 Simulation Workflow

```text
       Verilog RTL Design
               |
               v
        Verilog Testbench
               |
               v
         Synopsys VCS
               |
               v
         RTL Simulation
               |
               v
         FSDB Database
               |
               v
        Synopsys Verdi
               |
               v
        Waveform Analysis
```

### 7.2 VCS Compilation

Compile the RTL source and testbench using:

```bash
vcs -full64 -debug_access+all \
wallace_tree_multiplier.v \
wallace_tree_multiplier_tb.v \
-o simv
```

Ensure that the Verdi FSDB PLI integration is configured correctly in the VCS environment.

### 7.3 Execute Simulation

```bash
./simv
```

The simulation executes the testbench and generates the waveform database when the appropriate dump commands are enabled.

### 7.4 FSDB Waveform Generation

To generate an FSDB waveform, use the following commands inside the testbench:

```verilog
initial begin
    $fsdbDumpfile("wallacetreemultiplier.fsdb");
    $fsdbDumpvars(0, wallace_tree_multiplier_tb);
end
```

Ensure that the appropriate FSDB PLI support is enabled during VCS compilation.

### 7.5 Open FSDB in Synopsys Verdi

Use the following command to open the generated FSDB waveform:

```bash
verdi -ssf wallacetreemultiplier.fsdb &
```

The waveform database can be explored using the Verdi waveform viewer.

## 8. Waveform Analysis

Synopsys Verdi is used to inspect and analyze the simulation waveforms.

The following signals are observed:

| Signal | Width | Description |
|---|---:|---|
| `a` | 4 bits | Multiplicand input |
| `b` | 4 bits | Multiplier input |
| `p` | 8 bits | Final multiplication output |

Waveform analysis helps to:

- Inspect input and output signal transitions.
- Verify multiplication results for different input combinations.
- Analyze combinational circuit behavior.
- Debug signal-level functionality.
- Inspect the design hierarchy and internal signals.

## 9. Tools and Technologies

| Tool / Technology | Purpose |
|---|---|
| Verilog HDL | RTL design implementation |
| Synopsys VCS | RTL compilation and simulation |
| Synopsys Verdi | Waveform visualization and debugging |
| FSDB | Simulation waveform database |
| Git | Version control |
| GitHub | Source code management and documentation |

## 10. Key Learning Outcomes

Through this project, the following concepts are explored:

- Combinational digital circuit design.
- Binary multiplication architecture.
- Partial product generation.
- Wallace Tree reduction methodology.
- Half Adder and Full Adder implementation.
- Structural RTL modeling using Verilog.
- Testbench development and functional verification.
- RTL simulation using Synopsys VCS.
- Waveform analysis and debugging using Synopsys Verdi.
- FSDB waveform generation and inspection.

## 11. Future Enhancements

Potential extensions to this project include:

- Extending the Wallace Tree Multiplier to 8-bit and 16-bit architectures.
- Comparing Array Multiplier and Wallace Tree Multiplier architectures.
- Implementing Booth multiplication.
- Performing RTL synthesis and hardware area analysis.
- Evaluating propagation delay and critical paths.
- Developing a parameterized multiplier architecture.
- Performing gate-level simulation and timing verification.

## 12. Author

**SANTHOSH KUMAR N A**

RTL Design | Digital Design | VLSI Engineering

- GitHub: [Your GitHub Profile](https://github.com/your-username)
- LinkedIn: [Your LinkedIn Profile](https://linkedin.com/in/your-profile)

## 13. License

This project is intended for educational and learning purposes.

You are welcome to explore, modify and extend the implementation with appropriate attribution.

---

<p align="center">
  <b>Designed with Verilog HDL | Simulated with Synopsys VCS | Debugged with Synopsys Verdi</b>
</p>

<p align="center">
  ⭐ If you find this project useful, consider starring the repository!
</p>

---

## GitHub Topics

verilog · verilog-hdl · wallace-tree-multiplier · vlsi · rtl-design · digital-design · digital-arithmetic · synopsys-vcs · synopsys-verdi · fsdb · rtl-verification · asic-design · hardware-design
