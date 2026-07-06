# legendary-umbrella

A resonant half-bridge inverter designed to drive a custom high frequency step-up transformer for continuous high voltage supply.

## 🛠 Project Architecture
* **Front-End:** Standard Boost Active PFC (Targeting ~400V DC Bus, keeping reference ground quiet).

  <div align="center">
    <img src="https://media.monolithicpower.com/wysiwyg/11_6.png" width="1000" alt="Example of an Active PFC System">
  </div>
  
* **Inverter Topology:** Resonant Half-Bridge LLC (Utilizing The Leakage Inductance Of The Primary Coil).

  <div align="center">
    <img src="https://media.monolithicpower.com/wysiwyg/Articles/Fig_1_-_Half-Bridge_LLC_Resonant_Converter_with_a_Center-Tapped_Transformer.jpg" width="1000" alt="Example of a Half Bridge LLC Topology">
  </div>

  Where Q1 and Q2 are the primary switching elements, $L_r$ and $C_r$ are the Primary Resonant Inductor and Resonant Capacitor respectively, and $L_m$ is the   Magnetizing Inductance of the Primary Coil of the Transformer.

* **Control Loop:** Aiming for a Variable Frequency based Power Modulation or Zero Voltage Switching (ZVS) to reduce switching losses.

## 📓 Engineering Logs & Lessons Learned

### Log 1: Exploiting Transformer Flaws (LLC Tuning)
* **What I learned:**

  Instead of fighting Secondary Winding Capacitance and Primary Leakage Inductance, an LLC converter utilizes these parasitics into the tank model such as the Leakage Inductance of the Primary Coil of the Transformer, which I will refer to as $L_l$.
  
  * **At No Load:** Almost no power is being transferred across the transformer. Therefore, the Primary Coil will act as a Series Inductor with an Inductance value of $L_m$.
  * **Under Load:** The Primary Coil will act more as a Power Transfer element rather than an Inductor, bypassing $L_m$ and leaving only $L_l$ in series with the Primary Resonant Elements $L_r$ and $C_r$ to determine the natural Resonant Frequency $(f_r)$ of the Primary Resonant Tank.
* **The Math:**

$$\large
f_r=\frac{1}{2\pi \sqrt{C(L_r+L_l)}}
$$

### Log 2: Measuring the Inductances of individual Coils of the Transformer
* **What I learned:** Measuring $L_m$ of each coil can be achieved by probing the coil with an LCR meter. However, measuring $L_l$ requires the other coil(s) to be shorted to bypass the measurement of $L_m$.
* **The Math:**

  Let $k$ be the Coupling Coefficient of the Transformer. Therefore,
  
$$\large
L_l=L_m(1-k)
$$
