# Modular SSTC Schematic Sketch (Pinouts and Nets)

Overview
- Two-module SSTC system: Driver PCB (Module A) and External Coil Assembly (Module B).
- Clear isolation boundary between modules to improve safety, EMI containment, and maintainability.

Module A — Driver PCB (Module A)
- Responsibilities:
  - Microcontroller or SSTC controller with optional PLL for resonance tracking.
  - Isolated gate-drive stage for MOSFETs on Module B.
  - Isolated power rails for logic and gate drives (DC-DC converter).
  - Control and sensing interfaces (fault, ready, current/voltage sensing).
- Interfaces (to Module B):
  - Control link: PWM drive signals (GATE_Q1, GATE_Q2, GATE_Q3, GATE_Q4) via isolation barrier.
  - Enable: ENABLE signal to Module B.
  - Optional: PLL_LOCK signal if used.
  - Power link: a dedicated isolated DC-DC output (e.g., 5V or 12V) for gate drivers and control logic.
- Feedback connector: READY and FAULT are provided from Module B to Module A via a separate feedback connector.

Module B — External Coil Assembly (Module B)
- Responsibilities:
  - Primary coil: robust external winding or PCB-wrapped coil on an insulating form.
  - Secondary coil: mounted on a dedicated insulating form; top-load provided.
  - MOSFETs and gate-drive power: high-side/low-side devices arranged to form the inverter.
- Interfaces:
  - Gate-drive inputs: GATE_Q1, GATE_Q2, GATE_Q3, GATE_Q4 (isolated) from Module A.
  - HV power path: HV_BUS_POS and HV_BUS_NEG (external to Module A; via safe interconnect).
  - DC-DC gate-drive power: V_GD_ISO and GND_GD_ISO (isolated).
  - Diagnostics: READY, FAULT, TEMP_SENSE (optional) fed back to Module A.

- Pinouts (example)
 - J1 — 8-pin control link (Module A to Module B)
   - Pin 1: GATE_Q1
   - Pin 2: GATE_Q2
   - Pin 3: GATE_Q3
   - Pin 4: GATE_Q4
   - Pin 5: ENABLE
   - Pin 6: PLL_LOCK (optional)
   - Pin 7: (reserved)
   - Pin 8: (reserved)
- J2 — 2-pin power link (Module A to Module B)
   - Pin 1: V_GD_ISO (gate-driver supply)
   - Pin 2: GND_GD_ISO
- J3 — 2-pin feedback link (Module B to Module A)
   - Pin 1: READY
   - Pin 2: FAULT

Net naming conventions
- GATE_Qx nets drive the corresponding MOSFETs on Module B through the gate-drive stage.
- V_GD_ISO and GND_GD_ISO supply the isolated gate drive circuits on Module B.
- HV_BUS_POS/HV_BUS_NEG provide the high-voltage DC path to the primary coil.
- READY/FAULT propagate status for safe operation.

Example high-level schematic snippet (text)
  Module A MCU --[PLL/OSC]--> Module A Gate-Driver (isolated) --[GATE_Q1..Q4]--> Module B Gate-Driver/Buf --[GATE_Q1..Q4]--> Module B MOSFETs
  Module A --[V_GD_ISO]--> Module B Gate-Driver Power
  Module B --[HV_BUS_POS, HV_BUS_NEG]--> Primary Coil
  Module B --[Topload]--> Secondary Coil

Notes
- This is a starting schematic; detailed schematic diagrams, exact component values, and board layouts will be produced in subsequent iterations.

## ASCII schematic (textual netlist)
```text
Module A (Driver PCB)
  MCU/PLL Controller  ---- PWM_G1..PWM_G4 ---> Gate-Drive Isolator (Module B)
  Isolated 5V/12V DC-DC supply -> V_GD_ISO -> Gate-Drive Power
  ENABLE/READY/FAULT signals --------------+ 
                                         |
Module B (External Coil Assembly)
  Gate-Drive Receiver  --- GATE_Q1..GATE_Q4 ---> MOSFETs Q1..Q4
  HV_BUS_POS ----+                               |
  HV_BUS_NEG ----+                               |
  Primary Coil (external) connected to HV bus        \
  Secondary Coil with Topload on insulating form       > to resonance
 
Net mappings (example names)
- GATE_Q1, GATE_Q2, GATE_Q3, GATE_Q4  -> MOSFET gates
- V_GD_ISO, GND_GD_ISO               -> Gate-drive power
- HV_BUS_POS, HV_BUS_NEG             -> Primary coil power
- READY, FAULT                         -> status feedback
```


