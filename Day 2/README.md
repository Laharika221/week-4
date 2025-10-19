
# ⚡ Day 2 — Velocity Saturation & Basics of CMOS Inverter VTC  

---

## 🎯 Objective
Understand MOSFET behavior in **long-channel** vs **short-channel** devices and study the **CMOS inverter Voltage Transfer Characteristic (VTC)**.  

**Focus areas:**
- I–V characteristics of long and short channel MOSFETs  
- Velocity-saturation effects in short channels  
- Static CMOS inverter behavior and VTC derivation  

---

## 🔍 1. MOSFET I–V Characteristics  

### 1.1 ID vs VGS Curves  

We examined **drain current (ID)** versus **gate-to-source voltage (VGS)** for both long- and short-channel devices.

| Device Type | Behavior | Key Equation / Observation |
|--------------|-----------|----------------------------|
| **Long-channel MOSFET** | Exhibits **quadratic** dependence on VGS above threshold. | \( I_D = k_n (V_{GS} - V_T)^2 \) (saturation region). |
| **Short-channel MOSFET** | Follows quadratic law at first, then transitions to **linear** dependence due to **velocity saturation**. | Current saturates earlier because carriers reach maximum velocity. |

📈 **Observation:**  
Short-channel devices exhibit **early current saturation**, dominated by **velocity-limited carrier transport**.

---

### 1.2 Velocity Saturation — Definition  

**Velocity saturation** occurs when carrier drift velocity no longer increases linearly with electric field, but **saturates** at a maximum value \(v_{sat}\).  

| Electric Field | Carrier Velocity (v) | Region |
|----------------|----------------------|--------|
| Low | Linear increase | Mobility-controlled |
| High | Saturates to \(v_{sat}\) | Velocity-saturated |

📊 This effect is more prominent in **short-channel** devices and modifies the **ID–VGS** relationship from quadratic to near-linear.  

---

### 🧩 Operation Modes Comparison

In short channels, **drain current saturates earlier** than in long channels.  
This change directly influences switching speed and delay in CMOS circuits.  

---

## 🧠 2. MOS Device Characteristics  

| Condition | Device State | Resistance |
|------------|--------------|-------------|
| \(V_{GS} < V_T\) | OFF | Infinite |
| \(V_{GS} > V_T\) | ON | Finite |

### ⚙️ CMOS Structure Overview  

| Input (VIN) | Output (VOUT) | Path Active |
|--------------|---------------|-------------|
| 0 V | VDD | PMOS conducts |
| VDD | 0 V | NMOS conducts |

During transitions, both devices partially conduct, leading to **short-circuit currents**.

---

### 2.1 Node & Device Voltages  

\[
\begin{aligned}
V_{GSn} &= V_{in} - V_{SS} \\
V_{DSn} &= V_{out} \\
V_{GSp} &= V_{in} - V_{DD} \\
V_{DSp} &= V_{out} - V_{DD} \\
I_{DSp} &= - I_{DSn}
\end{aligned}
\]

These relationships define current flow through each transistor for a given input/output pair.

---

## ⚡ 3. NMOS & PMOS I–V Characteristics  

- **NMOS:** \( I_{dsn} vs V_{dsn} \) for various VGS.  
- **PMOS:** \( I_{dsp} vs V_{dsp} \), noting opposite polarity.  

These help in analyzing **pull-up (PMOS)** and **pull-down (NMOS)** strengths in the inverter.

---

## 🧮 4. CMOS Inverter VTC (V Transfer Characteristic)

### 4.1 Procedure to Derive VTC  

1. Identify operating regions (linear/saturation) of NMOS and PMOS.  
2. Apply **current equilibrium:** \( I_{dsn} = − I_{dsp} \).  
3. Solve for \(V_{OUT}\) as a function of \(V_{IN}\).  
4. Tabulate and plot **VTC** curve.

| VIN (V) | VOUT (V) | NMOS Region | PMOS Region |
|----------|-----------|-------------|-------------|
| 0.0 | VDD | OFF | Linear |
| 0.5 | 1.7 | Linear | Saturation |
| 1.0 | 1.0 | Saturation | Saturation |
| 1.5 | 0.3 | Saturation | Linear |
| 1.8 | 0 | Saturation | OFF |

---

### 4.2 Key Observations  

- **Long-channel:** Quadratic ID–VGS → classic VTC shape.  
- **Short-channel:** Velocity saturation → slightly flatter VTC slope.  
- **Sizing (W/L):** Adjusts inverter switching threshold \(V_m\).  

---

## 💡 5. Insights & Discussion  

| Concept | Impact |
|----------|--------|
| **Velocity Saturation** | Limits drain current → affects rise/fall times. |
| **CMOS Inverter as Switch** | Output depends on relative NMOS/PMOS strengths. |
| **Accurate I–V Models** | Crucial for predicting VTC and timing behavior. |

---

## 🧪 Day 3 – Lab Experiments  

### 🔹 Plot 1 — ID vs VDS (NFET)

**SPICE Deck:**  
```spice
* NFET ID–VDS Simulation
.include sky130_fd_pr__nfet_01v8__tt.pm3.spice
M1 drain gate source bulk nfet_01v8 L=0.18u W=1u
Vgs gate source 0 0.8
Vds drain source 0 2.5
.dc Vds 0 2.5 0.05
.plot dc I(M1)
.end
```
🔹 Plot 2 — ID vs VGS (NFET)

SPICE Deck:
```
.dc Vgs 0 2.5 0.05
.plot dc I(M1)
.end
```
🧩 Conclusion

Day 2 focused on comparing long vs short-channel MOSFETs.

- Introduced velocity saturation, explaining linear ID–VGS in short channels.

- Derived CMOS inverter VTC, establishing a base for switching threshold & noise-margin analysis.

✨ End of Day 2 — Velocity Saturation & CMOS Inverter VTC Analysis (Sky130 PDK)
