# 🧠 Day 3: CMOS Switching Threshold and Dynamic Simulation

## 📅 Previous Day Recap
On **Day 2**, I explored the **CMOS inverter fundamentals** and its **Voltage Transfer Characteristics (VTC)**, focusing on:

- Theoretical derivation of VTC curve  
- Behavior of **NMOS** and **PMOS** in inverter configuration  
- Understanding **switching threshold** conceptually  

---

## 🎯 Objective
The goal for **Day 3** was to perform **SPICE simulations** for the CMOS inverter and extract:
- **Switching threshold voltage (Vm)**
- **Rise time (tᵣ)** and **Fall time (t_f)**
- **Dynamic behavior** during switching

---

## ⚙️ 1. SPICE Simulation Setup

### 🧩 1.1 Netlist Overview
A SPICE deck defines:
- Device connections  
- Node naming and identification  
- Transistor **W/L ratios**  
- Supply voltage specifications  

**Example SPICE Netlist:**
- CMOS Inverter VTC Simulation
Vdd vdd 0 1.8
Vin in 0 0
M1 out in vdd vdd pmos W=2.5u L=0.15u
M2 out in 0 0 nmos W=1.5u L=0.15u
.dc Vin 0 1.8 0.1
.control
run
plot v(in) v(out)
.endc
.end


🖼️ *SPICE Netlist Screenshot (NgSpice)*

---

### ⚖️ 1.2 W/L Ratios Considered

| Case | PMOS (W/L) | NMOS (W/L) | Description |
|------|-------------|-------------|--------------|
| 1 | 1.5 | 1.5 | Equal strength |
| 2 | 2.5 | 1.5 | PMOS is stronger (balanced switching) |

---

## 📉 2. Voltage Transfer Characteristic (VTC) Simulation

### 🔬 2.1 Observations
**Case 1 (PMOS = NMOS = 1.5):**
- Switching threshold ≈ **0.99 V**
- VTC slightly skewed due to equal strengths  

**Case 2 (PMOS = 2.5, NMOS = 1.5):**
- Switching threshold ≈ **1.2 V**
- Balanced and symmetric transition

🖼️ *VTC Graphs (DC Sweep 0 V → 1.8 V, step = 0.1 V)*

---

## ⚡ 3. Transient Analysis (Dynamic Behavior)

### 🧠 3.1 Rise and Fall Delays

| Parameter | Definition |
|------------|-------------|
| **tᵣ (Rise time)** | Time for output to rise from 50% Vout to 50% Vin |
| **t_f (Fall time)** | Time for output to fall from 50% Vout to 50% Vin |

**Formulas:**
tᵣ = (50% of Vout) - (50% of Vin)
t_f = (50% of Vout) - (50% of Vin)

---

## ⚙️ 4. Switching Threshold — Theory & Derivation

At **Vm (Switching Point):**
- `Vin = Vout`
- Both NMOS and PMOS operate in **saturation**
- Direct current path between **VDD → GND**  
  → leads to short-circuit current

**Equating drain currents:**
I_DSn = -I_DSp

From this, the **Vm** can be derived based on device parameters and W/L ratios.

🧾 *Switching Threshold Derivation Diagram*

---

## 🧩 5. W/L Ratio Analysis

| Ratio | Observation |
|--------|--------------|
| (W/L)p = (W/L)n | Early switching threshold, asymmetric |
| (W/L)p = 2 × (W/L)n | Balanced rise/fall delay |
| (W/L)p = 3×–5× | Faster rise delay, ideal for data paths |

✅ **Optimal Ratio:**  
`(W/L)p ≈ 2 × (W/L)n` gives the **best balance** between power, speed, and symmetry.

---

## 🧪 6. Lab Work: Simulation Results

### ⚡ VTC Simulation (DC Sweep)
**Command:**
```bash
ngspice <filename.spice>


🖥️ Terminal Output Screenshot

Observation:

Switching threshold (Vm) ≈ 0.879 V

Symmetrical VTC → perfect for clock inverters

⏱️ Transient Simulation

Definition:
Transient analysis studies circuit behavior during transitions from one steady state to another.

SPICE Command:
```
.tran 1n 10n
```
(Step = 1 ns, Duration = 10 ns)

Observation:
When input toggles High → Low, output transitions Low → High, confirming inverter behavior.

### 📊 Rise and Fall Delay Results
| Parameter            | 50% Vout (ns) | 50% Vin (ns) | Delay (ns)    |
| -------------------- | ------------- | ------------ | ------------- |
| **Rise Delay (tᵣ)**  | 3.3536        | 2.1643       | **1.1893 ns** |
| **Fall Delay (t_f)** | 4.0548        | 2.4878       | **1.5670 ns** |

### ⚛️ 7. Connection to Device Physics

- During switching, both transistors conduct simultaneously, causing short-circuit current.

- Capacitive charging/discharging defines tᵣ and t_f.

- PMOS sizing (~2× NMOS) balances mobility (μn > μp).

- Steep VTC slope ↔ High switching speed.
### 🧮 8. STA (Static Timing Analysis) Correlation
| Parameter                    | STA Relation                         |
| ---------------------------- | ------------------------------------ |
| **Rise/Fall delay**          | Used in `.lib` cell delay tables     |
| **Switching threshold (Vm)** | Defines reference voltage (~50% VDD) |
| **Sizing & Vth variations**  | Affect setup/hold margins            |

### 🧾 9. Tabulated Summary
| Category              | Description / Observation                               |
| --------------------- | ------------------------------------------------------- |
| **Objective**         | SPICE simulation of CMOS inverter (VTC, Vm, tᵣ, t_f)    |
| **Tool**              | NgSpice with Sky130 PDK                                 |
| **VDD**               | 1.8 V                                                   |
| **DC Sweep**          | 0 V → 1.8 V (step = 0.1 V)                              |
| **Analysis Types**    | DC, Transient                                           |
| **W/L Ratios**        | (1.5/1.5), (2.5/1.5), up to (5×)                        |
| **Vm Range**          | 0.87 V – 1.2 V                                          |
| **Best Ratio**        | (W/L)p = 2 × (W/L)n                                     |
| **Transient Command** | `.tran 1n 10n`                                          |
| **tᵣ**                | 1.189 ns                                                |
| **t_f**               | 1.567 ns                                                |
| **Observation**       | Symmetrical transfer; inverter output complements input |
| **Insight**           | Larger PMOS improves rise delay                         |
| **Conclusion**        | Robust CMOS inverter with optimal (W/L)p ≈ 2 × (W/L)n   |

### 🧠 Conclusion

- Conducted detailed VTC and Transient analysis of CMOS inverter

- Determined switching threshold, rise/fall delays, and symmetry condition

- Verified that (W/L)p ≈ 2 × (W/L)n provides optimal performance

- Linked simulation results to device physics and timing analysis (STA)

💡 This forms a critical foundation for library characterization, timing modeling, and standard cell design in CMOS VLSI.

### 📁 Repository Contains:

- Day3_CMOS_Inverter.spice

- Simulation screenshots

- Terminal outputs

- VTC and Transient plots

🧩 Next Step → Day 4: CMOS Noise Margin and Power Dissipation Study
