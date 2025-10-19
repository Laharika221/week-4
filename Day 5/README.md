# 🧠 Day 5 – CMOS Power Supply and Device Variation Robustness Evaluation

## 🎯 Objective
To study how CMOS circuits behave under **power supply variations** and understand the impact of **voltage scaling** on gain, delay, and energy efficiency.  
Additionally, evaluate **device variation robustness** using SPICE simulations.

---

## 📚 Topics Covered

### 1️⃣ Introduction to Power Supply Variation
- Explored how changing the **supply voltage (VDD)** affects CMOS performance.
- Simulated behavior from **2.5 V → 0.5 V**.
- Even with lower voltages, CMOS continues to function, but **speed** and **robustness** degrade.

> The diagram shows the transition from **strong PMOS + weak NMOS** (high VDD)  
> to **strong NMOS + weak PMOS** (low VDD).

---

### 2️⃣ SPICE Simulation for Power Supply Variation
- Used a SPICE netlist that sweeps **VDD from 2.5 V → 0.5 V**.
- Commands between `.control` and `.endc` automate parameter sweeps or loops.

---

### 3️⃣ VTC Curve Analysis
- Observed Voltage Transfer Characteristics (VTC) for various VDD values.
- At **2.5 V** → inverter shows strong PMOS behavior.  
- At **0.5 V** → inverter shows strong NMOS behavior.

---

### 4️⃣ Why Not Always Use Low Supply Voltages (0.5 V)?
Though low voltages improve energy efficiency, they **increase delay** and reduce performance.

#### ⚙️ Gain Comparison
| Supply Voltage (VDD) | Gain | Improvement |
|----------------------:|------:|-------------:|
| 2.5 V | 7.38 | — |
| 0.5 V | 11.83 | ≈ 56 % |

> ✅ **Observation:** Gain improves at lower voltages (~56 % better at 0.5 V).

#### ⚡ Energy Comparison
| Supply Voltage (VDD) | Energy Expression | Relative Energy |
|----------------------:|------------------:|----------------:|
| 2.5 V | ½ C (2.5)² | 100 % |
| 0.5 V | ½ C (0.5)² | ≈ 4 % |

> ✅ **Observation:** ~96 % reduction in switching energy at 0.5 V.

#### ⏱️ Delay and Performance
| Parameter | 2.5 V | 0.5 V | Observation |
|------------|------:|------:|-------------|
| Rise Delay | 66 ps | Not sufficient to charge | Slow transition |
| Fall Delay | 78 ps | Much longer | Reduced performance |

> ⚠️ **Conclusion:** Very low VDD causes poor switching performance despite lower energy consumption.  
> Real chips use around **1–1.5 V** for optimal trade-off.

---

### ✅ Summary of Pros and Cons
| Advantages (Low VDD) | Disadvantages (Low VDD) |
|----------------------|--------------------------|
| ~56 % improvement in gain | Increased rise/fall delay |
| ~96 % reduction in power | Poor switching performance |
| Better energy efficiency | May not fully switch ON |
| Suitable for analog/low-power | Not practical for high-speed logic |

---

## 🧪 Day 5 – Lab 1: Power Supply Variation

- Conducted **SPICE-based supply variation** experiment.
- Simulated **VDD from 1.8 V → 0.8 V** (step = 0.2 V).
- Observed **VTC** and **gain**
### ⚡ Gain Values from Simulation
| VDD (V) | |Gain| |
|----------|:------:|
| 1.8 | 8.259 |
| 1.6 | 8.460 |
| 1.4 | 9.226 |
| 1.2 | 9.266 |
| 1.0 | 9.049 |
| 0.8 | 8.810 |

> 🔍 **Inference:** Gain increases as voltage decreases until 1 V,  
> after which it slightly drops — indicating an optimal operating region.

---

## 🧪 Day 5 – Lab 2: Device Variation Robustness

- Simulated CMOS inverter with **(W/L)\_P ≫ (W/L)\_N** (PMOS ≫ NMOS).
- Observed **VTC** stability even under severe size mismatch.

### ⚙️ Observations
- Despite large sizing difference, **VTC shape is maintained**.
- **Switching threshold ≈ 0.98 V**.
- CMOS inverter remains **robust** against device variations.

---

## 💡 Key Takeaways
- CMOS circuits are robust against **supply and device variations**,  
  but require **careful VDD and sizing optimization**.
- **Lower VDD → higher gain + lower energy**,  
  yet **slower transitions** and **weaker drive strength**.
- Understanding these effects ensures **reliable, low-power CMOS design** for modern VLSI systems.

---
