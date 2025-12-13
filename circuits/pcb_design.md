# PCB Design for a Solid-State Tesla Coil (SSTC)

## 1. Concept Overview

This document outlines the design for a compact, printed circuit board (PCB)-based Solid-State Tesla Coil (SSTC). Unlike traditional spark-gap Tesla coils, an SSTC uses semiconductor components (MOSFETs or IGBTs) to oscillate the primary coil, offering greater control, reliability, and a smaller form factor.

The goal is to create a single-PCB design that is easy to assemble, safe to operate (within limits), and serves as an educational tool for demonstrating wireless power transmission and high-frequency resonance.

## 2. Principle of Operation

The SSTC operates as a self-resonant system. The core components are a primary and secondary coil, forming an air-core resonant transformer.

1.  **Power Supply:** A low-voltage DC input (e.g., 12-24V) powers the entire circuit.
2.  **Driver Circuit:** A driver circuit, typically using a dedicated IC or a simple oscillator, switches power to the primary coil at a high frequency.
3.  **Primary Coil:** The primary coil is integrated into the PCB as a spiral trace. When the driver circuit sends pulses of current through it, it generates a magnetic field.
4.  **Secondary Coil:** A traditional, cylindrical secondary coil is mounted vertically in the center of the PCB. The magnetic field from the primary coil induces a high-frequency, high-voltage current in the secondary coil.
5.  **Resonance:** The driver circuit is tuned to the resonant frequency of the secondary coil, maximizing energy transfer and voltage gain. This results in a high-voltage, low-current plasma discharge from the top of the secondary coil.

## 3. Key Components and PCB Layout

The entire circuit will be housed on a single PCB, with the exception of the secondary coil and its topload.

### 3.1. Power Supply Section
*   **Input:** A standard DC barrel jack for a 12-24V power adapter.
*   **Filtering:** Large electrolytic capacitors to handle current spikes.
*   **Protection:** Reverse polarity protection diode and a fuse.

### 3.2. Driver Circuit
*   **Oscillator:** A 555 timer or a dedicated gate driver IC will be used to generate the high-frequency switching signal.
*   **MOSFETs/IGBTs:** Two transistors in a half-bridge or full-bridge configuration will switch the current to the primary coil.
*   **Heatsinks:** The switching transistors will require heatsinks, which should be accounted for in the PCB layout.

### 3.3. Primary Coil
*   **PCB Trace:** The primary coil will be a spiral trace etched directly onto the PCB. This simplifies construction and ensures consistency.
*   **Trace Width:** The trace width will be calculated to handle the primary current without overheating.

### 3.4. Secondary Coil and Topload
*   **Mounting:** The PCB will have a designated central area for mounting the cylindrical secondary coil.
*   **Connections:** Pads will be provided for soldering the secondary coil's base and ground connections.
*   **Topload:** A small, toroidal or spherical topload will be attached to the top of the secondary coil to increase its capacitance and shape the electric field.

## 4. Advantages of a PCB-Based Design

*   **Compactness:** Integrates all components onto a single board, making the device portable and easy to store.
*   **Simplicity:** Eliminates the need for complex, hand-wound primary coils and spark gaps.
*   **Reliability:** Solid-state components are more reliable and consistent than spark gaps.
*   **Safety:** Lower input voltage and the absence of a spark gap make this design safer for educational and hobbyist use.
*   **Reproducibility:** A PCB design can be easily manufactured, ensuring that each unit performs consistently.

## 5. Bill of Materials (BOM) - Preliminary

| Component             | Quantity | Description                                      |
| --------------------- | -------- | ------------------------------------------------ |
| DC Barrel Jack        | 1        | For power input                                  |
| Electrolytic Capacitor| 2        | 1000uF, 35V, for power filtering                 |
| 555 Timer IC          | 1        | Oscillator for the driver circuit                |
| MOSFETs               | 2        | e.g., IRF540N, for switching the primary coil    |
| Heatsinks             | 2        | For the MOSFETs                                  |
| Resistors, Capacitors | Various  | For the driver and timing circuits               |
| Secondary Coil        | 1        | Hand-wound or pre-made, ~1000 turns of fine wire |
| Topload               | 1        | Small metal sphere or toroid                     |

## 6. Safety Precautions

*   **High Voltage:** The secondary coil produces high-voltage, high-frequency AC. While less dangerous than mains AC, it can still cause RF burns.
*   **Ozone Production:** The plasma discharge will produce ozone. Use in a well-ventilated area.
*   **Electromagnetic Interference:** The device will generate significant EMI, which can interfere with nearby electronic devices.
