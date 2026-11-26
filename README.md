# Automated MCB Protection System (Hardware Architecture)

> **Project Status:** `Rev A — Schematic Proof-of-Concept` *(Schematic-level architecture; PCB layout and DFM cleanup pending for Rev B)*

## Overview
A galvanically isolated **Automated Miniature Circuit Breaker (MCB) controller** designed for fast overcurrent protection, automated load testing, and remote shunt-trip actuation. Built around an **ACS758 Hall-effect current sensor**, an **LM393 hardware window comparator**, and an **ESP32-WROOM-32** supervisory controller, this architecture mirrors the isolated current-sensing and fail-safe trip principles used in electric vehicle (EV) battery protection systems.

## Schematic
![Automated MCB Schematic](automated-mcb-schematic.png)

*(Download the full high-resolution vector schematic here: [automated-mcb-schematic.pdf](automated-mcb-schematic.pdf))*

## Key Engineering Highlights
* **Sub-Millisecond Hardware Trip:** Uses an `LM393` dual comparator with adjustable trip thresholds (`RV1`, `RV2`) directly connected to the `ACS758` current sensor output for deterministic overcurrent detection independent of microcontroller firmware latency.
* **Galvanic Isolation:** Separates the high-voltage sensing/switching ground (`GND`) from the microcontroller logic ground (`GND_MCU`) using high-speed `6N137` and `PC817` optocouplers.
* **Shunt-Trip & Test Relay Control:** Drives three switching/trip relays (`K1`–`K3`) via `BC547` transistor stages and a `74HC138` demultiplexer for automated MCB tripping and load diagnostics.

## Known Issues & Planned Fixes for Rev B
This repository currently contains the initial functional schematic (`Rev A`). Before PCB layout and fabrication, the following improvements are planned for `Rev B`:
* **Inductive Kickback Protection:** Add `1N4007` flyback diodes across relay coils `K2` and `K3` (currently implemented on `K1` only).
* **Base Current Limiting:** Add a `1 kΩ` base resistor between the `6N137` optocoupler output and transistor `Q1` (`BC547`).
* **Logic-Level Alignment:** Power the `74HC138` demultiplexer (`U5`) from `3.3V` instead of `5V` to match the native `ESP32` GPIO voltage domain.
* **Decoupling & Filtering:** Place `100 nF` ceramic decoupling capacitors across the `LM393`, `74HC138`, and `ACS758` power rails, and specify exact RC low-pass filter values (`R1`, `C1`) at the current sensor output.
