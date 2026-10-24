# 8-Bit Arithmetic Logic Unit (ALU) - Hierarchical Custom IC Design

##  Overview
This repository showcases the hierarchical custom layout and transistor-level design of an 8-bit Arithmetic Logic Unit (ALU). The project was designed and simulated using **Cadence Virtuoso** with the **gpdk090 (90nm) CMOS process technology**[cite: 4, 10]. 

*Note: Due to licensing constraints with the Cadence Generic Process Design Kit (gpdk090), the original design files (.oa, .il, etc.) are not uploaded. This repository contains the complete documentation, schematics, layout screenshots, and post-layout simulation results to demonstrate the design process and verification.*

##  Repository Structure
The project was built using a hierarchical, cell-based design approach. The folders are organized exactly as they were structured in the Cadence environment:

*   **`Basic_Gates/`**: Contains the schematic, symbol, and layout of fundamental gates (e.g., XOR, Inverter, NAND) designed from the transistor level.
*   **`Tri-state_Buffer/`**: Documentation for the custom transmission gates used for bus output control.
*   **`Full_Adder/`**: The 1-bit Full Adder (FA1X1) instantiated using the basic logic gates.
*   **`8-bit_Ripple_Carry_Adder/`**: The 8-bit addition engine created by cascading eight 1-bit Full Adders.
*   **`ALU_8bit/`**: The final integrated module containing the schematic, layout, and simulation waveforms of the complete 8-bit ALU.

##  Project Highlights
*   **Scale:** Designed a complex datapath integrating approximately **360+ transistors**[cite: 5].
*   **Methodology:** Employed a hierarchical design flow. Basic logic cells were created at the transistor level and then instantiated (reused) to build larger functional blocks like Full Adders and the complete ALU[cite: 5].
*   **Architecture:** Implemented an 8-bit Ripple Carry Adder, Subtraction Logic via an XOR bank (2's complement), and Tristate Buffer output control for safe W-bus interfacing[cite: 5].
*   **Verification:** Successfully passed **Design Rule Check (DRC)** and **Layout Versus Schematic (LVS)** checks[cite: 4, 10].
*   **Performance (Post-Layout Simulation):**
    *   **Worst-case Propagation Delay (Critical Path):** 4.771 ns[cite: 5]
    *   **Maximum Theoretical Frequency:** ~209 MHz[cite: 5]
    *   **Average Power Consumption:** 91.28 µW[cite: 5]

---

##  Design Flow & Architecture

### 1. Basic Logic Gates & Symbol Generation
The design started from the absolute basics. Fundamental logic gates (Inverter, NAND, XOR) were designed at the transistor level using NMOS and PMOS devices from the gpdk090 library[cite: 4, 10]. Symbols were generated for these gates to allow for hierarchical design reuse[cite: 5].

Inverter/Pre_Layout/schematic.png

### 2. The 8-Bit Ripple Carry Adder
Eight custom 1-bit Full Adders (FA1X1) were instantiated and cascaded to create the 8-bit Ripple Carry Adder. The carry-out of each stage was strictly tied to the carry-in of the subsequent stage[cite: 5, 8].

*(Insert a high-resolution screenshot of the 8-bit Ripple Carry Adder schematic)*
`![8-Bit RCA Schematic](Insert_Image_Link_Here)`

### 3. Complete ALU Architecture
To convert the adder into a full ALU capable of both addition and subtraction without using multiplexers:
*   An array of 8 XOR gates was instantiated to conditionally invert the 'B' input, creating a 1's complement[cite: 5, 8].
*   A single control signal (`alu_sub`) was tied to the second input of all XOR gates and directly to the $C_{in}$ of the first adder. This efficiently added the "+1" required to complete the 2's complement subtraction logic[cite: 5, 8].
*   The final sum output was routed through an 8-bit Tristate Buffer controlled by the `alu_out_en` pin to ensure a high-impedance state when the ALU is not driving the CPU data bus[cite: 5, 8].

*(Insert a screenshot of the final ALU schematic showing the Adder, XOR bank, and Buffers)*
`![Final ALU Schematic](Insert_Image_Link_Here)`

### 4. Hierarchical Custom Layout
Instead of drawing the entire ALU polygon-by-polygon, the layout was generated hierarchically using the pre-designed basic cell layouts. The integrated layout was verified to be 100% DRC and LVS clean[cite: 4, 10].

*(Insert a clear screenshot of the final 8-bit ALU layout)*
`![Full Custom Layout](Insert_Image_Link_Here)`

### 5. Post-Layout Transient Simulation & Power Analysis
The `av_extracted` view was generated to include parasitic resistances and capacitances[cite: 4]. 
*   **Functionality:** Transient analysis was run using Cadence ADE L. 
*   **Delay & Power:** The critical path delay was determined by rippling a carry bit through all 8 stages, resulting in a maximum delay of 4.771 ns[cite: 5]. Average power was calculated as 91.28 µW over a 100ns simulation window using the Virtuoso Calculator[cite: 5].

*(Insert a screenshot of the ADE L waveform showing the transient analysis)*
`![Simulation Waveform](Insert_Image_Link_Here)`

##  Tools Used
*   **Schematic Entry:** Cadence Virtuoso Schematic Editor L[cite: 4]
*   **Simulation:** Cadence Analog Design Environment (ADE L - Spectre)[cite: 4]
*   **Layout Verification:** Assura DRC and LVS[cite: 4]
*   **Technology Node:** Cadence gpdk090 (90nm)[cite: 4, 10]
