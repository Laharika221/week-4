
# 🌿 Day 1 – Ngspice Sky130 Basics  
### *NMOS Drain Current (ID) vs Drain-to-Source Voltage (VDS)*  

---

## 🎯 Objective
Understand how the **drain current (ID)** varies with **drain-to-source voltage (VDS)** in an NMOS transistor using **Ngspice** and **Sky130 PDK**, and explore how SPICE modeling supports CMOS circuit design.

---

## ⚙️ Introduction to SPICE Simulation

SPICE (Simulation Program with Integrated Circuit Emphasis) is the backbone of transistor-level circuit analysis.  
We began by exploring the **Ngspice** engine and discussing three key questions posed by *Kunal Ghosh Sir*:

1. Where does the delay of a cell actually come from?  
2. The delay tables we use — where do they originate?  
3. How can we verify if STA timing data (from synthesis or GDS) is accurate?

These questions highlight why SPICE is essential: it connects **transistor physics → delay modeling → STA accuracy**.

---

## 🧩 NMOS Structural Overview

**NMOS** is a four-terminal device with:

| Terminal | Symbol | Description |
|-----------|---------|-------------|
| Gate | G | Controls channel conductivity |
| Drain | D | Collects carriers |
| Source | S | Supplies carriers |
| Body | B | Substrate connection |

**Physical composition:**
- P-type substrate  
- N+ diffusion (Source & Drain)  
- Polysilicon gate  
- Gate oxide  
- Isolation regions  

🧠 *Note:* The substrate (body) is often grounded and influences the threshold voltage (Vt).

---

## ⚡ Threshold Voltage (Vt)

- When **VGS = 0**, both source-substrate and drain-substrate junctions behave as PN diodes → channel OFF.  
- Increasing **VGS** attracts electrons, forming an **inversion layer**.  
- The voltage where this inversion supports conduction is **Vt (threshold voltage)**.

### Body Effect (Substrate Bias)
Two conditions:
- **VSB = 0** → Normal Vt  
- **VSB > 0** → Higher Vt due to increased depletion width  

📐 **Equation:**  
\[
V_T = V_{T0} + \gamma(\sqrt{2\phi_f + V_{SB}} - \sqrt{2\phi_f})
\]

---

## 🧠 Modes of Operation

| Region | Condition | Description | Current Equation |
|--------|------------|--------------|------------------|
| Cut-off | \(V_{GS} < V_T\) | Channel OFF | \(I_D ≈ 0\) |
| Linear / Triode | \(V_{GS} > V_T, V_{DS} < (V_{GS} - V_T)\) | Acts as resistor | \(I_D = K'(W/L)[(V_{GS}-V_T)V_{DS}-V_{DS}^2/2]\) |
| Saturation | \(V_{DS} > (V_{GS}-V_T)\) | Channel pinch-off | \(I_D = ½K'(W/L)(V_{GS}-V_T)^2(1+λV_{DS})\) |

🌀 **Pinch-off Phenomenon:**  
Occurs when \(V_{GS}-V_{DS} < V_T\). The channel vanishes near the drain.

---

## ⚙️ Current Mechanisms

| Type | Cause | Description |
|------|--------|-------------|
| **Drift current** | Potential difference | Movement under electric field |
| **Diffusion current** | Carrier gradient | Movement from high to low concentration |

Total \(I_D\) = charge density × carrier velocity.

---

## 🧪 SPICE Simulation Insight

SPICE computes **ID vs VDS** by voltage sweeping:

1. For each fixed **VGS**, sweep **VDS** from 0 → (VGS–Vt).  
2. Plot **ID vs VDS**.  
3. Beyond (VGS–Vt), the device enters **saturation**.

✅ This simulation directly answers: *“How do we calculate ID for different VGS values?”*

---

## 📊 Key SPICE Model Parameters (Sky130)

| Parameter | Description |
|------------|-------------|
| **VTO** | Threshold voltage |
| **γ (gamma)** | Body-bias coefficient |
| **Kn′** | Process transconductance |
| **λ (lambda)** | Channel-length modulation |

---

## 🧾 Typical SPICE Netlist Example

```spice
M1 Vdd n1 0 0 nmos L=1.8u W=1.2u
R1 in n1 55
Vdd vdd 0 2.5
Vin in 0 2.5
.dc Vds 0 1.8 0.05
.plot dc I(M1)
.end

### 🧰 Day 1 Lab: NMOS ID–VDS Simulation
🧩 Step 1: Install Ngspice
```
sudo apt install ngspice
```
## Step 2: Explore Sky130 Repo
```
- nfet_01V8

- pfet_01V8
```

## 📚 Important files:
```
- sky130_fd_pr__nfet_01v8__tt.pm3.spice → constants (λ, γ, etc.)

- sky130_fd_pr__nfet_01v8__tt.corner.spice → (W/L) ratios
```
