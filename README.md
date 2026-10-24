# 8-Bit Arithmetic Logic Unit (ALU) - Hierarchical Custom IC Design

##  Overview
This repository showcases the hierarchical custom layout and transistor-level design of an 8-bit Arithmetic Logic Unit (ALU). The project was designed and simulated using **Cadence Virtuoso** with the **gpdk090 (90nm) CMOS process technology**. 

*Note: Due to licensing constraints with the Cadence Generic Process Design Kit (gpdk090), the original design files (.oa, .il, etc.) are not uploaded. This repository contains the complete documentation, schematics, layout screenshots, and post-layout simulation results to demonstrate the design process and verification.*

##  Repository Structure
The project was built using a hierarchical, cell-based design approach. The folders are organized exactly as they were structured in the Cadence environment:

*   **`Basic_Gates/`**: Contains the schematic, symbol, and layout of fundamental gates (e.g., NOR, OR, Inverter, NAND, MUX_2x1).
*   **`Derived_Gates/`**: Contains gates like XOR, which were built hierarchically using the pre-designed basic gates (e.g., AND, OR, Inverter) rather than from the direct transistor level.
*   **`Tri-state_Buffer/`**: Documentation for the custom transmission gates used for bus output control.
*   **`Full_Adder/`**: The 1-bit Full Adder (FA1X1) instantiated using the basic logic gates.
*   **`8-bit_Ripple_Carry_Adder/`**: The 8-bit addition engine created by cascading eight 1-bit Full Adders.
*   **`ALU_8bit/`**: The final integrated module containing the schematic, layout, and simulation waveforms of the complete 8-bit ALU.

## 🚀 Project Highlights
*   **Scale:** Designed a complex datapath integrating approximately **360+ transistors**[cite: 1].
*   **Methodology:** Employed a hierarchical design flow. Basic logic cells were created at the transistor level and then instantiated (reused) to build larger functional blocks like XOR gates, Full Adders, and the complete ALU[cite: 1].
*   **Architecture:** Implemented an 8-bit Ripple Carry Adder, Subtraction Logic via an XOR bank (2's complement), and Tristate Buffer output control for safe W-bus interfacing[cite: 1].
*   **Verification:** Successfully passed **Design Rule Check (DRC)** and **Layout Versus Schematic (LVS)** checks[cite: 1].

---

## 🛠️ Design Flow & Architecture

### 1. Basic Logic Gates & Symbol Generation
The design started from the absolute basics. Fundamental logic gates (Inverter, NAND, AND, OR) were designed at the transistor level using NMOS and PMOS devices from the gpdk090 library[cite: 1]. Symbols were generated for these gates to allow for hierarchical design reuse[cite: 1].

*(Example: Inverter Schematic and Symbol)*
![Inverter Schematic](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Pre_Layout/schematic.png)

### 2. Derived Gates (XOR)
Instead of building the XOR gate directly from MOSFETs, it was constructed hierarchically by instantiating the previously designed basic gates (AND, OR, and Inverters)[cite: 1]. This demonstrates a standard cell-based design approach.

*(Example: Hierarchical XOR Gate Schematic)*
`![XOR Schematic](Insert_Image_Link_Here)`

### 3. The 8-Bit Ripple Carry Adder
Eight custom 1-bit Full Adders (FA1X1) were instantiated and cascaded to create the 8-bit Ripple Carry Adder. The carry-out of each stage was strictly tied to the carry-in of the subsequent stage[cite: 1].

*(Insert a high-resolution screenshot of the 8-bit Ripple Carry Adder schematic)*
`![8-Bit RCA Schematic](Insert_Image_Link_Here)`

### 4. Complete ALU Architecture
To convert the adder into a full ALU capable of both addition and subtraction:
*   An array of 8 XOR gates was instantiated to conditionally invert the 'B' input, creating a 1's complement[cite: 1].
*   A single control signal (`alu_sub`) was tied to the second input of all XOR gates and directly to the $C_{in}$ of the first adder. This efficiently added the "+1" required to complete the 2's complement subtraction logic[cite: 1].
*   The final sum output was routed through an 8-bit Tristate Buffer controlled by the `alu_out_en` pin to ensure a high-impedance state when the ALU is not driving the CPU data bus[cite: 1].

*(Insert a screenshot of the final ALU schematic showing the Adder, XOR bank, and Buffers)*
`![Final ALU Schematic](Insert_Image_Link_Here)`

### 5. Hierarchical Custom Layout
Instead of drawing the entire ALU polygon-by-polygon, the layout was generated hierarchically using the pre-designed basic cell layouts. The integrated layout was verified to be 100% DRC and LVS clean[cite: 1].

*(Insert a clear screenshot of the final 8-bit ALU layout)*
`![Full Custom Layout](Insert_Image_Link_Here)`

---

## ⚡ Post-Layout Simulation & Performance Analysis
The `av_extracted` view was generated to include parasitic resistances and capacitances[cite: 1]. Transient analysis was run using Cadence ADE L to evaluate the true physical performance of the ALU.

*   **Worst-case Propagation Delay (Critical Path):** **4.771 ns**[cite: 1]
    *   *Methodology:* This was calculated by forcing a worst-case ripple scenario. A pulse signal was applied to the LSB of Input B (`alu_in_2<0>`) while Input A was set to all 1s (1111 1111). This forces a carry bit to ripple through all 8 full-adder stages. The delay was measured using the Virtuoso Calculator from the rising edge of the input pulse to the rising edge of the final `alu_carry_out` pin[cite: 1].
*   **Maximum Theoretical Frequency:** **~209 MHz**[cite: 1]
    *   *Methodology:* Derived using the formula $f_{max} = 1 / \text{Critical Path Delay}$ ($1 / 4.771\text{ ns}$)[cite: 1].
*   **Average Power Consumption:** **91.28 µW**[cite: 1]
    *   *Methodology:* Calculated using the `average(getData("/pwr" ?result "tran"))` function in the Virtuoso Calculator over a 100ns transient simulation window, capturing the dynamic power dissipation during both addition and subtraction operations[cite: 1].

*(Insert a screenshot of the ADE L waveform showing the transient analysis)*
`![Simulation Waveform](Insert_Image_Link_Here)`

## 💻 Tools Used
*   **Schematic Entry:** Cadence Virtuoso Schematic Editor L[cite: 1]
*   **Simulation:** Cadence Analog Design Environment (ADE L - Spectre)[cite: 1]
*   **Layout Verification:** Assura DRC and LVS[cite: 1]
*   **Technology Node:** Cadence gpdk090 (90nm)[cite: 1]
