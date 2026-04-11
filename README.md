# SandVault - Prototype Validation 🔋

This repository contains the physical validation firmware and technical documentation for the SandVault prototype I developed during the AlgoFest Hackathon 2026.

I successfully validated three critical fronts: Thermodynamic Engineering, Firmware Telemetry, and Predictive Economic Logic.

---

## 🛠️ 1. Physical Prototype & System Architecture

I designed the prototype to demonstrate real-world heat transfer from a solid storage medium (sand) to a liquid exchanger (water) for residential heating.

### Material Breakdown (BOM)
* **Storage Unit:** Two nested metal pots with high-density Rock Wool insulation.
* **Medium:** 13 kg of dry sand with an embedded Copper-to-Silicone heat exchanger.
* **Heating System:** * 2x 26W Soldering-tip resistors (230V AC).
    * 1x 50W Submerged resistor (12V DC).
* **Fluid Dynamics:** 1x 12V Water Pump for active circulation.
* **Control Hub:** ESP32 Microcontroller, 2x DS18B20 Sensors, and a 230V-to-12V Transformer.

### 🔌 Circuitry & Power Distribution
I powered the system using two separate lines to manage different power requirements:
* **High-Voltage AC (230V):** Powered the core resistors and the transformer.
* **Low-Voltage DC (12V):** Fed from the transformer to power the pump and auxiliary resistor.
* **Manual Control Strategy:** Due to relay communication challenges during assembly, I pivoted to a "Manual Switching Strategy". I manually toggled the 12V pump and resistors by connecting/disconnecting jumper wires on the breadboard and cycling the 230V sockets. This allowed me to successfully record the thermal rise while manually managing electrical safety.

---

## 💻 2. Firmware Logic (ESP32)

The `sensor_plotter.ino` file is the diagnostic heart of my prototype, providing the telemetry I required to prove the thermodynamic formulas.

### Technical Implementation:
* **Protocol:** I used the 1-Wire bus to read multiple DS18B20 sensors through GPIO 5.
* **Hardware Fix:** I integrated a 4.7kΩ pull-up resistor between Data and 3.3V to neutralize electromagnetic interference (EMI) from the nearby 230V heating elements.
* **Telemetry:** The code outputs a formatted string (Key:Value) at 115200 baud, enabling real-time visualization via the Arduino Serial Plotter.
* **Validation:** This firmware successfully recorded the water loop rising from 20°C to 45°C, proving the efficiency of the sand-to-copper heat transfer.

---

## 🧠 3. Smart Manager: Predictive Logic & Thermodynamics

The SandVault "brain" I designed is an energy-arbitrage engine. I validated the system's viability by integrating the physics of heat storage with market volatility.

### 🌡️ The Thermodynamics of Storage
To calculate the State of Charge (SoC) and autonomy, my algorithm uses:
**Q = m * c * Delta_T**

Where:
* **m = 13 kg** (Mass of sand).
* **c = 830 J/kg°C** (Specific heat capacity).
* **Delta_T = (Sand Temp) - (Target Water Temp).**

Thermal Autonomy (ta) is then calculated as: **ta = Q_available / Power_demand**.

### 📉 Algorithm Decision States:

1. **Critical State (Safety):** If SoC < 10% or autonomy < 2h, I programmed the system to force a grid charge regardless of price to guarantee service.
2. **Predictive State (OMIE Price Arbitrage):** My algorithm scans 72h weather forecasts. If a solar deficit is predicted, it schedules a charge at **03:00 AM** (statistically the lowest OMIE market price) to lock in low rates.
3. **Standby State (Efficiency):** When Solar Power > Demand, the grid is bypassed (Grid Power = 0), achieving 100% self-consumption.

---

## 🔗 Project Links
* **Interactive Dashboard (Figma):** [Open SandVault Dashboard](https://www.figma.com/make/xtBx5ii2K2D1QqXKYnANKD/SandVault---Dashboard?fullscreen=1&t=4Nc4OFI8Wy0oCBo6-1)
