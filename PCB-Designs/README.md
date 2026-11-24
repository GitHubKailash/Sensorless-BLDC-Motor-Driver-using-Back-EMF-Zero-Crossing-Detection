# Sensorless BLDC Motor Drive – 4-Layer PCB Design

## Project Overview

This project focuses on the design and development of a **Sensorless Brushless DC (BLDC) Motor Drive** using a custom-designed 4-layer PCB. The system is designed to control BLDC motor operation without requiring Hall-effect sensors by utilizing Back Electromotive Force (Back-EMF) detection.

The controller integrates the microcontroller and three-phase motor driver circuitry onto a single PCB, providing a compact and integrated motor control solution.

## Key Features

- Sensorless BLDC motor control using Back-EMF detection.
- Three-phase inverter-based motor driving circuit.
- Integrated microcontroller-based control system.
- Four-layer PCB design for improved routing and power distribution.
- USB-based programming interface.
- 12V DC power supply.
- Designed for A2212 13T 1000KV BLDC motor.

## Hardware Specifications

| Parameter | Specification |
|---|---|
| Motor Type | Brushless DC Motor |
| Motor Model | A2212 13T |
| Motor Speed Constant | 1000 KV |
| Operating Supply | 12V DC |
| Control Technique | Sensorless Control |
| Feedback Method | Back-EMF Detection |
| PCB Layers | 4 Layers |
| Design Software | KiCad |

## PCB Design

The PCB is designed using KiCad with a four-layer architecture to accommodate the control circuitry, power electronics, and signal routing.

The four-layer structure provides:
- Improved power and ground distribution.
- Better signal routing.
- Reduced interference between control and power sections.
- Compact component integration.

## Working Principle

1. The microcontroller generates the switching signals required for three-phase motor operation.
2. The inverter circuit drives the BLDC motor through its three-phase windings.
3. Back-EMF signals are monitored from the non-energized motor phase.
4. Zero-crossing detection is used to determine rotor position.
5. The controller performs electronic commutation based on the detected rotor position.
6. This process continues to maintain motor rotation without Hall-effect sensors.

## Tools and Technologies

- KiCad – Schematic Capture and PCB Design
- Embedded C / Arduino IDE – Firmware Development
- Back-EMF Detection – Sensorless Rotor Position Estimation
- Four-Layer PCB Technology – Hardware Implementation

## Repository Contents

- `Schematic/` – Circuit schematic files
- `PCB/` – Four-layer PCB layout files
- `Gerber/` – PCB manufacturing files
- `BOM/` – Bill of Materials
- `Images/` – PCB layout and design images
- `README.md` – Project documentation

## Applications

- Drone BLDC motor drives
- Robotics and automation
- Electric propulsion systems
- Embedded motor control applications

## Future Improvements

- Closed-loop speed regulation.
- Improved startup and low-speed performance.
- Current and temperature monitoring.
- Protection against overcurrent and undervoltage.
- PCB testing and performance optimization.

## Author

Designed and developed as an Electrical and Electronics Engineering project.

**Project:** Sensorless BLDC Motor Drive  
**PCB Design:** 4-Layer Custom PCB  
**Design Tool:** KiCad
