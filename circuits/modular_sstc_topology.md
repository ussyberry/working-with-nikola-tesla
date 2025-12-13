# Modular SSTC Topology Sketch (Driver PCB + External Coil Assembly)

Overview
- Proposes a two-package SSTC design: a driver/control PCB and a separate external coil assembly containing the power stage.
- Enables safer isolation, easier maintenance, and future upgrades while preserving resonant behavior.

Driver PCB (Module A)
- Responsibilities:
  - Power management: isolated supply for gate drives and control logic.
  - Control logic: microcontroller or dedicated SSTC controller with optional PLL for resonance tracking.
  - Gate-drive stage: isolated gate drivers with dead-time control and protection.
  - Signal conditioning and sensing: current sensing, voltage sensing, fault protection.
- Interfaces:
  - Control link to External Coil Assembly: opto-isolated or transformer-isolated GPIO/PWM signals.
  - Power link: isolated DC-DC outputs to gate drivers and control circuits.
  - Safety interlocks and EMI filtering on the input.

External Coil Assembly (Module B)
- Responsibilities:
  - Primary coil: robust coil form (could be winding on a non-conductive bobbin or PCB-wrapped with insulation) driven by the driver module.
  - Secondary coil: mounted on a dedicated insulating form; topload provided to shape resonance.
  - Gate driver power and MOSFETs: high-side/low-side devices arranged to form the resonant inverter (e.g., bridge or half-bridge) as dictated by the design.
- Interfaces:
  - High-voltage power path: from the driver-included power interface to the MOSFETs.
  - Control link: receive PWM/drive signals from Driver PCB.
  - Safety and shielding: physical separation, enclosure, and EMI considerations.

Proposed topologies
- Topology 1: Full-bridge inverter on External Coil Assembly
  - Four MOSFETs arranged in a full-bridge driven by isolated gate signals from the Driver PCB.
  - Primary coil wound as a robust external winding or PCB trace on a separate substrate with proper insulation.
- Topology 2: Half-bridge with shared return (simpler) on External Coil Assembly
  - Two MOSFETs, gate signals isolated; suitable for smaller power levels and easier cooling.

Key interfaces (high-level)
- Driver PCB to External Coil Assembly:
  - PWM/gate drive signals (isolated)
  - Power for gate drivers (isolated DC-DC)
  - Optional status/diagnostic data
- Coil Assembly power path:
  - Main DC bus to MOSFETs
  - Ground reference separated by isolation barrier

Drafted guiding principles
- Isolation: enforce galvanic isolation between modules for safety and EMI containment.
- Modularity: design for swapping coil assemblies or driver revisions without reworking the other module.
- Safety: enclosure, creepage/clearance, and EMI shielding documented from the outset.

Notes
- This sketch is a starting point; detailed schematic diagrams, board layouts, and thermal analyses should be produced in subsequent iterations.

## Testing and validation plan
- Thermal: place temperature sensors on MOSFET(s) and critical components; run under simulated load, record temperature rise, compare to datasheet limits.
- EMI/EMC: perform basic conducted and radiated EMI checks near the design; implement shielding and layout adjustments as needed to reduce emissions.
- Isolation tests: verify barrier integrity with leakage-current tests and hipot-style tests in a safe lab; ensure isolation rating margins are respected.
- Functional tests: validate PWM signaling, gate-drive timing (dead-time), and resonance tracking at a chosen target frequency.
- Safety checks: verify enclosure integrity, creepage/clearance compliance, shielding effectiveness, and grounding strategy.

## Expanded details (next steps)
- Module A (Driver PCB) should include:
  - Isolated power rails for gate drives and control logic.
  - Controller block with optional PLL for resonance tracking.
  - Isolated gate-drive stage with dead-time protection and fault handling.
  - Sensing: current, voltage, temperature, and fault status.
- Module B (External Coil Assembly) should include:
  - Primary coil rigidly mounted on a non-conductive form or external winding with insulation.
  - Secondary coil mounted on an insulating form; top-load and shielding features as needed.
  - MOSFETs and gate drive power arranged for safe, reliable operation; consider heat sinking and airflow.
- Interfaces between modules:
  - PWM/drive signals (isolated) and a dedicated control link.
  - Isolated power feed to gate-drive circuitry.
  - Diagnostics: READY/FAULT, temperature, current limit feedback.
- Topologies considerations:
  - Full-bridge inverter on Module B (External Coil Assembly) for robust performance.
  - Alternative: half-bridge with a split supply if heat or layout constraints require it.
- Safety, EMI, and enclosure planning:
  - Creepage/clearance according to standards, shielding on the coil, and a secure enclosure.
  - EMI considerations including shielding, filtering, and layout practices.

