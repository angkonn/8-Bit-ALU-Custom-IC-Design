# CMOS Inverter - Custom IC Design

##  Overview
This directory contains the custom transistor-level design, simulation, and verification of a standard CMOS Inverter. It is a fundamental building block for the 8-bit ALU project. The design is implemented using **Cadence Virtuoso** and the **gpdk090 (90nm) CMOS process technology**.

##  Design Highlights
*   **Technology Node:** Cadence gpdk090 (90nm)
*   **Design Type:** Full-custom transistor-level design
*   **Transistor Sizing:** 
    *   **PMOS:** Width = 480nm, Length = 100nm
    *   **NMOS:** Width = 240nm, Length = 100nm
*   **Verification:** Passed **DRC** and **LVS** checks.

---

##  Pre-Layout Design & Simulation

### 1. Schematic Design
The inverter was designed at the transistor level using `nmos1v` and `pmos1v` devices from the gpdk090 library. The PMOS width was set to twice the NMOS width ($W_p = 2W_n$) to ensure balanced rise and fall times, considering the mobility difference between holes and electrons.

*(Inverter Schematic)*
![Inverter Schematic](8-Bit-ALU-Custom-IC-Design/Inverter/Pre_Layout/schematic.png)

### 2. Symbol Generation
A custom symbol was created for the inverter to allow for hierarchical design reuse in larger circuits (e.g., XOR gate, Full Adder).

*(Inverter Symbol)*
![Inverter Symbol](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Pre_Layout/symbol.png)

### 3. Pre-Layout Transient Simulation
Transient analysis was performed using **Cadence ADE L** to evaluate the logical functionality and timing characteristics. A pulse voltage source was applied to the input, and the output correctly demonstrated logic inversion.

*(Pre-Layout Simulation Waveform)*
![Inverter Waveform](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Pre_Layout/waveform.png)

####  Pre-Layout Performance Metrics
The Virtuoso Calculator was used to measure the delay and power from the transient simulation data.

*   **Propagation Delay:** **39.91 ps (39.91E-9 s)**
    *   *Methodology:* Measured from the 50% point of the input rising edge to the 50% point of the output falling edge (Threshold = 0.6V).
    *   ![Delay Calculation](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Pre_Layout/delay.png)
*   **Average Power Consumption:** **2.958 µW (2.958E-6 W)**
    *   *Methodology:* Calculated using the `average(getData(":pwr" ?result "tran"))` function over the simulation window.
    *   ![Power Calculation](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Pre_Layout/avg_power.png)

---

##  Post-Layout Verification & Analysis

### 1. Custom Layout
The layout was manually routed using Metal 1 for interconnects, Poly for the gate, and appropriate N-well/P-sub contacts. The design was optimized to maintain a constant standard cell height for integration into the larger ALU datapath.

*(Inverter Layout)*
![Layout](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Post_Layout/layout.png)

### 2. DRC & LVS Check
*   **Design Rule Check (DRC):** Passed (0 Errors). The layout strictly adheres to the gpdk090 technology spacing and sizing rules.
*   **Layout Versus Schematic (LVS):** Passed. The extracted netlist perfectly matches the schematic.

### 3. Post-Layout Transient Simulation (av_extracted)
The `av_extracted` view was generated using Assura Quantus QRC to include parasitic resistances and capacitances contributed by the physical routing. 

*(av_extracted View)*
![av_extracted](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Post_Layout/av_extracted.png)

####  Post-Layout Performance Metrics
*   **Propagation Delay:** **39.72 ps (39.72E-9 s)**
    *   ![Post-Layout Delay Calculation](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Post_Layout/delay_2.png)
*   **Average Power Consumption:** **2.003 µW (2.003E-6 W)**
    *   ![Post-Layout Power Calculation](https://raw.githubusercontent.com/angkonn/8-Bit-ALU-Custom-IC-Design/main/Basic_Gates/Inverter/Post_Layout/avg_power_2.png)

---

##  Pre-Layout vs. Post-Layout Comparison

| Metric | Pre-Layout (Schematic) | Post-Layout (Extracted) | Deviation |
| :--- | :--- | :--- | :--- |
| **Propagation Delay** | 39.91 ps | 39.72 ps | ~0.47% decrease |
| **Average Power** | 2.958 µW | 2.003 µW | ~32.28% decrease |

*Analysis:* The very small deviation in propagation delay indicates an optimized, clean layout with minimal parasitic capacitance on the critical path. The difference in average power consumption is within expected variations for post-layout parasitic extraction, likely due to a more realistic modeling of the device and interconnect parasitics in the `av_extracted` view compared to the ideal schematic simulation.
