# nano critique of pcb_design.md in circuits

This document provides a focused critique of `circuits/pcb_design.md`, specifically addressing feasibility of the PCB-based SSTC concept and the existence of similar products.

## Feasibility concerns
- The design proposes a PCB-mounted primary coil and an integrated secondary coil arrangement. In practice, the secondary coil typically requires a non-conductive bobbin or form and significant insulation/creepage distances from the PCB. Attempting to mount a tall, cylindrical secondary on/over a single PCB is unlikely to be mechanically robust or thermally reliable.
- The primary coil as a PCB spiral raises thermal and current-density concerns. High-current switching traces must carry peak currents that exceed what a single thin PCB layer can safely handle, even with heatsinking considerations. Real-world designs usually use a separate, robust coil structure and place power electronics on a dedicated, well-ventilated board.
- The driver section mentions a 555 timer or a dedicated gate driver IC. For reliable SSTC operation, a proper high-side/low-side gate drive with dead-time control, shoot-through protection, and adequate isolation is typically required. A bare 555-based approach is unlikely to tolerate high dV/dt stress and may risk MOSFET failure.
- Power supply guidance (12–24 V) suggests a low-voltage source powering the entire system. To achieve substantial resonance voltage on the secondary, a higher DC bus and robust HV switching topology (often 400–600 V or higher with appropriate heat sinking and protection) are commonly used. The document lacks clarity on how high voltages are generated and regulated within safe limits.
- Safety and EMI considerations are acknowledged but underspecified. The design would generate significant RF emissions and ozone during plasma discharge. Without a documented enclosure, shielding strategy, creepage/clearance planning, and EMI mitigation, safe educational use is questionable.
- The BOM and component choices appear optimistic for a compact, single-board approach. In practice, sourcing high-voltage MOSFETs, gate drivers, high-current capacitors, and insulation materials will require careful selection and validation, not just a nominal bill of materials.
- Overall, the concept as written is ambitious for a single PCB with integrated coil structures. A more realistic path is to separate the coil assembly from the driver PCB, use a proven driver topology, and clearly document isolation, cooling, and safety enclosures.

## Existing products and landscape
- Commercial turnkey SSTC products are rare due to safety and regulatory concerns. The market is dominated by hobbyist projects and open-source designs rather than mass-produced PCB modules.
- Open-source SSTC projects and driver boards exist, typically pairing a dedicated driver module with a separate, well-insulated coil assembly. These designs emphasize modularity (driver board + coil form) and robust HV isolation, which this PCB-centric approach lacks.
- If the goal is a teaching/demo platform, a safer and more practical route is to use a separate high-voltage coil assembly with a tested driver PCB, enabling easier maintenance, tuning, and safety audits.

## Recommendations (high level)
- Reconsider the physical topology: move the secondary coil off the PCB onto an insulated non-conductive form; keep the PCB for driver electronics and safe interconnects.
- Replace the 555-based oscillator with a proper SSTC driver topology that includes dead-time, current sensing, and robust gate control.
- Define clear insulation, creepage, and clearance specs, and include a documented enclosure and EMI shielding plan.
- Validate power architecture with a realistic HV bus and current/power budgets; perform thermal analysis and, if possible, simulation of resonant behavior before building.
- Provide references to existing SSTC designs or products to anchor the project in proven approaches and set realistic expectations.

## Final note
This critique emphasizes feasibility over novelty. If you want to pursue a PCB-first SSTC, a modular, driver-first approach with a separate coil assembly and documented safety/EMI considerations will increase realism and educational value.

## Expanded modular topology fleshed-out (concrete)
- Driver PCB module (Module A) should house:
  - Isolated power rails for logic and gate drives (e.g., DC-DC converter providing 5V/12V with isolation).
  - Controller block with optional PLL for resonance tracking.
  - Isolated gate-drive stage with dead-time and shoot-through protection.
  - Fault and status reporting (FAULT, READY, HIGH-VENT/OVERCURRENT indicators).
- External coil assembly (Module B) should house:
  - Primary coil on a robust insulating form or external winding; separate from digital logic to ease cooling and insulation.
  - Secondary coil mounted on an insulated, non-conductive form with a defined creepage/clearance path to the primary side.
  - Topload and shielding elements to shape the electric field and contain EMI.
- Interfaces between Module A and Module B:
  - Galvanically isolated PWM/gate-drive signals (prefer transformer or optical isolation) with defined timing margins.
  - Isolated power feed for gate drivers and any analog sensing on Module B, or a shared isolated supply via a DC-DC converter.
  - Status/diagnostic channels (FAULT/ALARM, temperature) to enable safe operation.
- Power and thermal considerations:
  - Clearly budgeted HV bus (for the coil) and separate lower-voltage rails for the driver.
  - Thermal management design for MOSFETs and driver electronics; include heatsinking and ventilation paths.
- Safety and EMI plan:
  - Enclosure with creepage/clearance compliant gaps; EMI shielding around the external coil; grounding strategy.
- Open-source references (expanded):
  - Loneoceans SSTC4 (modular driver + coil) – https://loneoceans.com/labs/sstc4/
  - Hackaday: Single Mosfet Mini SSTC – https://hackaday.io/project/195337-single-mosfet-mini-sstc-tesla-coil-with-10-cm
  - Lu Labs: PLL SSTC concepts – https://lulabs.net/tesla/sstc2/
  - Hackaday: Programmable SSTC – https://hackaday.io/project/182198-programmable-sstc
  - (Additional) SSTC design references from community docs on safe coil assemblies.
- Draft checklist for next phase:
  - Produce a detailed schematic for Module A and Module B with pins, nets, and isolation strategy.
  - Create a minimal BOM outline including a driver IC, isolated DC-DC, MOSFETs, and coil form components.
  - Validate creepage/clearance distances and enclosure dimensions.

## Concrete references to open-source SSTC designs
- Loneoceans Labs SSTC4 design (modular driver + coil approach): https://loneoceans.com/labs/sstc4/
- Single Mosfet Mini SSTC (MOSFET-centric, emphasizes cooling): https://hackaday.io/project/195337-single-mosfet-mini-sstc-tesla-coil-with-10-cm
- PLL-based SSTC testbed (frequency stabilization via PLL): https://www.lulabs.net/tesla/sstc2/
- Programmable SSTC (open-source, programmable driver): https://hackaday.io/project/182198-programmable-sstc

> These references illustrate modularity, gate-drive robustness, and safety-focused design choices that can inform a more realistic SSTC implementation.

## Revised topology sketch (modular)
- Propose a driver PCB that contains the control logic, PLL (if used), oscillator, and isolated gate-driver circuitry.
- Use an isolated power supply for gate drives and any analog sensing circuitry.
- External coil assembly contains:
  - Primary coil (could be a robust external winding or a PCB-wrapped coil on an insulating form)
  - Secondary coil mounted on an insulated form with a top-load
  - High-voltage interconnects to the driver via a galvanically isolated interface (e.g., opto/isolation transformer)
- Gate drives from the driver PCB to MOSFETs in the external coil assembly, with dead-time and shoot-through protection implemented on the driver.
- EMI shielding, creepage/clearance planning, and enclosure design documented for safe educational use.

Drafted topology sketch file: `circuits/modular_sstc_topology.md` (see next section).

