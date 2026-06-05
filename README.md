# Fluid Power Systems — Hydraulic & Pneumatic Circuit Design

Design and simulation of hydraulic and pneumatic control systems for industrial applications using **FluidSIM** software. Covers single- and double-acting cylinders, directional control valves, solenoid-operated circuits with ladder diagrams, and electro-pneumatic relay logic with sensor-based automation.

> **Course:** Hydraulic & Pneumatic Control (MEC2307)  
> **Institution:** Helwan National University — Robotics & Mechatronics Department  
> **Supervisor:** Dr. Ahmed Kadry  
> **Team:** Yousef Ahmed Elbeltagy · Mohamad Sherif Shabrawy

---

## Part 1 — Hydraulic Circuits (FluidSIM-H)

Three industrial hydraulic applications designed and simulated. Each application has two parts: **(a)** manual lever-actuated valve and **(b)** solenoid-operated valve with electrical ladder diagram.

---

### Application 1 — Hardening Furnace Cover

A **single-acting cylinder** raises and lowers a furnace cover. A **10 kg load (98.1 N)** is attached to the cylinder rod — it simulates the real weight of the cover and acts as the natural return force when hydraulic pressure is released.

| Part | Valve | Control |
|---|---|---|
| (a) | 3/2 lever-actuated, spring-return | Manual lever |
| (b) | 3/2 solenoid-operated, spring-return | Pushbutton + solenoid Y + ladder diagram |

**Operation:**
- **Extend (Cover Raises):** Lever/pushbutton actuated → valve shifts P→A → pressurized fluid enters cylinder → piston extends upward against the 10 kg load
- **Retract (Cover Lowers):** Lever/button released → spring returns valve A→T → pressure relieved → gravity (10 kg load) pulls piston back down

> Load configured in FluidSIM-H as a constant force of 98.1 N (F = m × g = 10 × 9.81)

---

### Application 2 — Furnace Door Control

A **double-acting cylinder** opens and closes a furnace door. The spring-return valve provides fail-safe behavior — the door automatically closes when the valve is released, which is critical for furnace safety.

| Part | Valve | Control |
|---|---|---|
| (a) | 4/2 lever-actuated, spring-return | Manual lever |
| (b) | 4/2 solenoid-operated, spring-return | Pushbutton + solenoid Y + ladder diagram |

**Operation:**
- **Extend (Door Opens):** Lever/button actuated → valve shifts P→B, A→T → fluid enters rod side → piston extends, door opens
- **Retract (Door Closes):** Lever/button released → spring returns valve P→A, B→T → fluid enters blank end → piston retracts, door closes automatically

---

### Application 3 — U-Shape Bending Device

A **double-acting cylinder** performs a U-shape bending operation on sheet metal. Uses a **4/2 solenoid valve without spring return** — the valve holds its position until the opposite solenoid is energized. Two independent pushbuttons control each stroke, and **flow control valves** regulate speed on both ports.

| Component | Description |
|---|---|
| Valve | 4/2 solenoid (X and Y) — no spring return |
| Pushbutton E1 | Energizes solenoid X → advance stroke (bending) |
| Pushbutton E2 | Energizes solenoid Y → return stroke |
| Flow control valve (Port A) | Regulates advance stroke speed |
| Flow control valve (Port B) | Regulates return stroke speed |

**Operation:**
- **Advance (Bending):** Press E1 → solenoid X energized → P→A, B→T → fluid flows (regulated) into blank end → cylinder extends, bends workpiece
- **Return:** Press E2 → solenoid Y energized → P→B, A→T → fluid flows (regulated) into rod side → cylinder retracts to original position

---

## Part 2 — Pneumatic Circuits (FluidSIM-P)

Two industrial pneumatic applications designed and simulated — from basic pushbutton control to a fully automated electro-pneumatic system.

---

### Application 1 — Industrial Transport System

A **single-acting pneumatic cylinder** pushes goods from a storage shelf onto a transfer belt. Simple, reliable, and suitable for high-frequency repetitive operations.

| Component | Description |
|---|---|
| Cylinder | Single-acting — extends under air pressure, retracts via internal spring |
| Valve | 3/2 pushbutton (normally closed), spring-return |
| FRL Unit | Filter–Regulator–Lubricator |

**Operation:**
- **Extend (Push Goods):** Pushbutton pressed → valve shifts port 1→2 → compressed air flows through FRL into cylinder → piston extends, pushes goods onto belt
- **Retract (Reset):** Pushbutton released → valve returns port 2→3 (exhaust) → air vented to atmosphere → internal spring retracts piston automatically

---

### Application 2 — Automatic Pneumatic Door Control

A **fully automated electro-pneumatic door system** using a double-acting cylinder with sensor-based relay logic. Detects people, holds the door open safely, and closes automatically after a 5-second delay.

| Component | Description |
|---|---|
| Cylinder | Double-acting — opens and closes door under air pressure |
| Valve | 5/2 directional control, dual-solenoid (X and Y) |
| Solenoid Y | Energizes to extend cylinder → door opens |
| Solenoid X | Energizes to retract cylinder → door closes |
| Optical proximity sensor | Detects person approaching the door |
| End-position sensor (S) | Confirms cylinder is fully extended before timer starts |
| Timer (T) | 5-second delay before door closes |
| Relay Y | Keeps door open and blocks timer while person is detected |
| Relay X | Activates return stroke after timer finishes |
| Power Supply | 24V DC |

**Sequence of Operation:**

```
1. Person detected  → proximity sensor ON → relay Y energized → solenoid Y ON
                     → valve shifts → cylinder extends → door OPENS
                     → timer BLOCKED (door stays open safely)

2. Person still present → relay Y active → door HOLDS OPEN

3. Person leaves    → proximity sensor OFF → relay Y de-energized
                     → end-position sensor (S) confirms full extension
                     → timer T starts (5-second delay)

4. Timer counts 5s  → door remains open during delay (safe exit time)

5. Timer finishes   → relay X energized → solenoid X ON
                     → valve reverses → cylinder retracts → door CLOSES

6. System resets    → end-position sensor resets → timer resets → ready for next cycle
```

---

## Repository Structure

```
fluid-power-systems-fluidsim/
├── hydraulics/
│   ├── circuits/
│   │   ├── app1a_furnace_cover_manual.ct        # App 1 — single-acting, 3/2 lever valve
│   │   ├── app1b_furnace_cover_solenoid.ct      # App 1 — solenoid + ladder diagram
│   │   ├── app2a_furnace_door_manual.ct         # App 2 — double-acting, 4/2 lever valve
│   │   ├── app2b_furnace_door_solenoid.ct       # App 2 — solenoid + ladder diagram
│   │   ├── app3_bending_device.ct               # App 3 — U-shape bending, dual solenoid + flow control
│   │   ├── double_acting_cylinder_demo.ct       # Reference circuit — double-acting cylinder
│   │   ├── single_acting_cylinder_demo.ct       # Reference circuit — single-acting cylinder
│   │   └── sorting_device_demo.ct               # Reference circuit — sorting device
│   └── docs/
│       └── Hydraulics_Report.pdf                # Full report: design, simulation, and analysis
├── pneumatics/
│   ├── circuits/
│   │   ├── app1_transport_system.ct             # App 1 — single-acting, 3/2 pushbutton valve
│   │   ├── app2_door_control.ct                 # App 2 — automatic door, relay logic
│   │   └── app2_door_control_v2.ct              # App 2 — revised version
│   └── docs/
│       └── Pneumatics_Report.pdf                # Full report: design, simulation, and analysis
└── README.md
```

> **Software:** FluidSIM-H (hydraulics) and FluidSIM-P (pneumatics) — `.ct` files open directly in FluidSIM

---

## Tools & Concepts

- **FluidSIM-H / FluidSIM-P** — Circuit design and simulation
- **Hydraulic components:** Single/double-acting cylinders, 3/2 and 4/2 directional control valves, flow control valves, HPU (motor + pump + relief valve)
- **Pneumatic components:** Single/double-acting cylinders, 3/2 and 5/2 valves, FRL units, proximity sensors, end-position sensors, timers
- **Control methods:** Manual lever actuation, solenoid-operated valves, ladder diagrams, relay logic
