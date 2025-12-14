# Tesla Resonance Charger - Circuit Design Considerations

## 1. Overview

This document outlines the initial considerations for the resonant circuit design of the Tesla Resonance Charger. The primary goal is to achieve efficient wireless power transfer over a specified range while adhering to safety and regulatory requirements.

## 2. Resonant Circuit Topology

We will primarily focus on **Series-Series (SS) compensated resonant inductive coupling** due to its advantages in maintaining high efficiency over varying coupling coefficients and load conditions. Other topologies (e.g., Series-Parallel, Parallel-Series, Parallel-Parallel) will be considered during optimization.

### 2.1. Transmitter (Tx) Resonant Circuit

*   **Components:**
    *   **High-Frequency Inverter:** To generate the high-frequency AC signal. A full-bridge or half-bridge inverter topology will be considered, driven by a microcontroller with PWM capabilities.
    *   **Resonant Inductor (L_Tx):** The primary coil responsible for generating the oscillating magnetic field. Design considerations include coil geometry (e.g., helical, spiral, planar), wire type (Litz wire for reduced skin effect), and number of turns.
    *   **Resonant Capacitor (C_Tx):** Tuned with L_Tx to achieve the desired resonant frequency. High-Q, low-ESR capacitors are essential.
    *   **Matching Network:** To match the impedance of the inverter to the resonant tank, maximizing power transfer efficiency.

### 2.2. Receiver (Rx) Resonant Circuit

*   **Components:**
    *   **Resonant Inductor (L_Rx):** The secondary coil designed to capture energy from the oscillating magnetic field. Similar design considerations as L_Tx.
    *   **Resonant Capacitor (C_Rx):** Tuned with L_Rx to resonate at the same frequency as the Tx circuit.
    *   **Rectifier:** To convert the received AC power to DC. A high-efficiency full-wave rectifier (e.g., Schottky diodes or synchronous rectification) will be used.
    *   **DC-DC Converter/Regulator:** To provide a stable and regulated DC output voltage to the connected device.
    *   **Load Matching Network:** To ensure efficient power delivery to the device.

## 3. Operating Frequency Selection

*   **Initial Range:** kHz to MHz. Specific frequency will be chosen based on:
    *   **Efficiency:** Higher frequencies generally allow for smaller coils but can increase losses due to skin effect and radiation.
    *   **Safety:** Compliance with human exposure limits (SAR values).
    *   **Regulatory Standards:** Adherence to ISM (Industrial, Scientific, and Medical) bands to minimize interference with other wireless systems.
    *   **Component Availability:** Availability of high-performance components at the chosen frequency.

## 4. Coil Design and Geometry

*   **Factors to Consider:**
    *   **Coupling Coefficient (k):** Maximizing k for efficient power transfer. Influenced by coil size, distance, and alignment.
    *   **Quality Factor (Q):** High Q factors for both Tx and Rx coils are crucial for efficient resonance.
    *   **Wire Type:** Litz wire to minimize AC resistance at high frequencies.
    *   **Shielding:** Magnetic shielding to confine the magnetic field and reduce interference.
    *   **Multi-axis Coils:** For 3D spatial freedom, multiple coils or a complex coil geometry may be required for the transmitter.

## 5. Control and Feedback System

*   **Microcontroller:** To manage:
    *   **Frequency Tuning:** Adaptive frequency tuning to maintain resonance under varying load and coupling conditions.
    *   **Power Control:** Adjusting output power based on detected devices and their charging needs.
    *   **Safety Monitoring:** Over-current, over-voltage, over-temperature, and Foreign Object Detection (FOD).
    *   **Communication:** BLE module for communication with devices and potentially a user interface.
*   **Feedback Mechanisms:** Current and voltage sensing on both Tx and Rx sides to provide feedback for control algorithms.

## 6. Safety and EMC Considerations

*   **FCC/CE Compliance:** Design must meet electromagnetic compatibility standards to prevent interference.
*   **Human Exposure Limits:** Ensure that EMF levels are well within safe limits for continuous human exposure.
*   **Foreign Object Detection (FOD):** Implement robust FOD mechanisms to prevent heating of metallic objects.
*   **Thermal Management:** Adequate heat sinking and thermal design for all power components.

## 7. Initial Component Selection (Preliminary)

*   **Microcontroller:** ARM Cortex-M series (e.g., STM32) for its processing power and peripheral set.
*   **Inverter MOSFETs:** High-frequency, low-Rds(on) MOSFETs.
*   **Resonant Capacitors:** Polypropylene film capacitors for high-frequency applications.
*   **Rectifier Diodes:** Schottky diodes for low forward voltage drop.
*   **Litz Wire:** For resonant coils.

## 8. Simulation Tools

*   **Electromagnetic Simulation Software:** ANSYS Maxwell, COMSOL Multiphysics, or similar for coil design and field analysis.
*   **Circuit Simulation Software:** LTSpice, PSpice, or similar for resonant circuit analysis and optimization.
