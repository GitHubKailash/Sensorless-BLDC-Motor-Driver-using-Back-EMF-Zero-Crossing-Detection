# Sensorless BLDC Motor Drive

<p align="center">
  <b>A Low-Cost Sensorless Three-Phase BLDC Motor Controller Using Arduino UNO, IR2104S Gate Drivers, and MOSFET-Based Power Electronics</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Arduino%20UNO-blue?style=for-the-badge&logo=arduino" />
  <img src="https://img.shields.io/badge/Language-Embedded%20C%2FC%2B%2B-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/PCB%20Design-KiCad-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Control-Sensorless%20BEMF-purple?style=for-the-badge" />
</p>

---

# Overview

The **Sensorless BLDC Motor Drive** is an Arduino UNO-based three-phase Brushless DC (BLDC) motor controller designed to operate a BLDC motor without requiring external Hall-effect position sensors.

The system uses **Back Electromotive Force (BEMF) zero-crossing detection** to determine the rotor position and perform electronic commutation. An Arduino UNO processes the BEMF signals and generates the required PWM control signals to operate six power MOSFETs through three IR2104S half-bridge gate driver ICs.

The controller implements a six-step electronic commutation technique to drive the three-phase BLDC motor. Motor speed is controlled using PWM, with dedicated push buttons for increasing and decreasing the motor speed.

The project was initially implemented and tested on a perfboard to verify the hardware functionality. Following successful prototype development, the complete motor driver circuit was designed as a PCB using **KiCad**, providing a dedicated hardware implementation.

The project focuses on developing an affordable, compact, and sensorless BLDC motor controller while exploring embedded systems, power electronics, motor control algorithms, and PCB design.

---

# Problem Statement

Conventional sensored BLDC motor controllers require Hall-effect sensors to determine the rotor position for electronic commutation.

Although these sensors provide rotor position information, they increase hardware complexity, wiring requirements, and overall system cost.

This project addresses these limitations by developing a sensorless BLDC motor controller that estimates rotor position using the Back Electromotive Force generated in the motor windings.

By eliminating the need for external rotor position sensors, the system reduces hardware requirements while implementing electronic commutation and PWM-based speed control using an Arduino UNO.

---

# Project Objectives

- Develop a sensorless BLDC motor controller using Arduino UNO.
- Implement BEMF-based rotor position detection.
- Perform six-step electronic commutation.
- Design a three-phase MOSFET inverter circuit.
- Interface IR2104S gate drivers with the Arduino controller.
- Implement PWM-based motor speed control.
- Provide manual speed adjustment using push buttons.
- Develop and test the complete circuit on a perfboard.
- Design a dedicated PCB using KiCad.
- Understand the practical implementation of embedded motor control and power electronics.

---

# Key Features

- Sensorless BLDC motor operation.
- Back EMF zero-crossing detection.
- Six-step electronic commutation.
- Three-phase inverter architecture.
- Arduino UNO-based control system.
- Three IR2104S half-bridge gate drivers.
- Six N-channel power MOSFETs.
- PWM-based speed control.
- Manual speed adjustment using push buttons.
- Open-loop startup sequence.
- Closed-loop BEMF-based commutation after startup.
- Perfboard-based hardware prototype.
- Custom PCB schematic and design using KiCad.

---

# System Architecture

The system consists of an Arduino-based control unit, a three-phase MOSFET inverter, gate driver circuits, and a sensorless BLDC motor.

```text
                  12V DC SUPPLY
                       |
                       |
              +------------------+
              |  Power Inverter  |
              |                  |
              |  6 Power MOSFETs |
              +------------------+
                       |
                       |
              +------------------+
              |  Three-Phase     |
              |  BLDC Motor      |
              +------------------+
                       |
             Phase A, B and C
                       |
                       v
              +------------------+
              |  BEMF Detection  |
              |  Voltage Divider |
              |  Network         |
              +------------------+
                       |
                       v
              +------------------+
              |   Arduino UNO    |
              |   ATmega328P     |
              |                  |
              |  BEMF Detection  |
              |  Commutation     |
              |  PWM Generation  |
              |  Speed Control   |
              +------------------+
                       |
                       v
              +------------------+
              |  IR2104S Gate    |
              |  Driver ICs      |
              +------------------+
                       |
                       v
              +------------------+
              |  MOSFET Switching|
              |  Control         |
              +------------------+
                       |
                       v
                 BLDC Motor
```

### Main System Blocks

| Block | Function |
|---|---|
| Arduino UNO | Main controller and commutation logic |
| BEMF Detection Network | Detects rotor position using motor-generated voltage |
| IR2104S Gate Drivers | Drive high-side and low-side MOSFETs |
| MOSFET Inverter | Converts DC supply into three-phase switched output |
| BLDC Motor | Converts electrical energy into mechanical rotation |
| Speed Control Buttons | Increase and decrease motor speed |

---

# Working Principle

The controller operates using the Back Electromotive Force generated by the three motor windings.

Unlike sensored BLDC controllers, this system does not depend on Hall-effect sensors to determine rotor position.

## 1. Back EMF Generation

When a BLDC motor rotates, its stator windings generate Back Electromotive Force (BEMF).

The three motor phases generate BEMF waveforms that are electrically displaced by 120 degrees.

During six-step commutation:

- Two motor phases are energized.
- One motor phase remains floating.
- The floating phase is monitored for BEMF zero crossing.

The zero-crossing information is used to determine the appropriate time for the next commutation event.

## 2. Virtual Neutral Point Generation

Three resistors are connected to the three motor phases to generate a virtual neutral reference point.

This reference voltage is compared with the BEMF voltage of the floating motor phase.

The voltage divider network reduces the motor phase voltage to a suitable level for the Arduino's detection circuitry.

## 3. BEMF Zero-Crossing Detection

The Arduino UNO uses its internal analog comparator to detect the zero-crossing events.

The comparator compares:

- Positive input: Virtual neutral reference.
- Negative input: Selected floating-phase BEMF signal.

When the floating-phase BEMF crosses the virtual neutral voltage, the comparator output changes state.

The Arduino detects this transition using the analog comparator interrupt.

## 4. Six-Step Electronic Commutation

The motor is controlled using six-step electronic commutation.

During each commutation step, one high-side MOSFET and one low-side MOSFET are activated while the third phase remains floating.

| Step | High-Side MOSFET | Low-Side MOSFET | Floating Phase |
|---|---|---|---|
| 1 | A | B | C |
| 2 | A | C | B |
| 3 | B | C | A |
| 4 | B | A | C |
| 5 | C | A | B |
| 6 | C | B | A |

The sequence repeats continuously to maintain motor rotation.

## 5. Open-Loop Startup

At standstill, the motor does not generate sufficient BEMF for reliable rotor position detection.

Therefore, the controller initially uses an open-loop startup sequence.

During startup:

1. The Arduino initializes the PWM output.
2. A predefined commutation sequence is applied.
3. The commutation interval gradually decreases.
4. The motor accelerates.
5. Once sufficient BEMF is available, the controller enables comparator interrupt-based commutation.

## 6. PWM-Based Speed Control

Motor speed is controlled by adjusting the PWM duty cycle applied to the gate driver control signals.

Two push buttons are provided:

- **Speed Up:** Increases the PWM duty cycle.
- **Speed Down:** Decreases the PWM duty cycle.

The controller maintains the duty cycle within predefined minimum and maximum limits.

---

# Hardware Components

| S.No | Component | Quantity | Purpose |
|---|---|---:|---|
| 1 | Arduino UNO | 1 | Main controller |
| 2 | IR2104S Gate Driver IC | 3 | Half-bridge gate driving |
| 3 | N-Channel Power MOSFET | 6 | Three-phase inverter |
| 4 | 1N4148 Diode | 3 | Bootstrap charging |
| 5 | 33kΩ Resistor | 6 | BEMF sensing and virtual neutral network |
| 6 | 10kΩ Resistor | 3 | Voltage divider network |
| 7 | 100Ω Resistor | 6 | MOSFET gate resistors |
| 8 | 10µF Capacitor | 3 | Gate driver supply decoupling |
| 9 | 2.2µF Capacitor | 3 | Bootstrap capacitors |
| 10 | Push Buttons | 2 | Speed adjustment |
| 11 | BLDC Motor | 1 | Motor load |
| 12 | 12V DC Supply | 1 | Power source |
| 13 | Perfboard | 1 | Prototype implementation |
| 14 | PCB | 1 | Custom circuit implementation |

**Note:** The MOSFET part number should be finalized according to the actual hardware revision. The supplied original reference schematic shows 06N03LA, while your KiCad schematic shows IRF3205.

---

# Controller and Pin Configuration

The Arduino UNO uses its ATmega328P microcontroller for PWM generation, BEMF detection, and motor commutation.

| Arduino Pin | Function |
|---|---|
| D3 | Phase A control |
| D4 | Phase B control |
| D5 | Phase C control |
| D6 | Virtual neutral reference (AIN0) |
| D7 | Phase A BEMF detection (AIN1) |
| A2 | Phase B BEMF detection |
| A3 | Phase C BEMF detection |
| D9 | Phase A gate driver PWM/SD control |
| D10 | Phase B gate driver PWM/SD control |
| D11 | Phase C gate driver PWM/SD control |
| A0 | Speed Up button |
| A1 | Speed Down button |

---

# Gate Driver Circuit

The project uses three IR2104S half-bridge gate driver ICs to control the six MOSFETs.

Each IR2104S controls one motor phase and provides:

- High-side MOSFET gate control.
- Low-side MOSFET gate control.
- Bootstrap-based high-side gate drive.
- Independent gate driver supply and switching connections.

The three gate driver circuits are connected to the corresponding Phase A, Phase B, and Phase C inverter stages.

Each phase consists of:

- One high-side N-channel MOSFET.
- One low-side N-channel MOSFET.
- One IR2104S gate driver.
- Associated bootstrap and gate resistor components.

---

# Software Implementation

The firmware is developed using the Arduino IDE in Embedded C/C++.

The program implements the following functions:

1. GPIO initialization.
2. PWM timer configuration.
3. Analog comparator configuration.
4. Initial open-loop motor startup.
5. BEMF zero-crossing detection.
6. Interrupt-based commutation.
7. Six-step switching sequence.
8. PWM duty cycle adjustment.
9. Push-button speed control.

### PWM Generation

The Arduino UNO uses its hardware timers to generate PWM signals.

The PWM outputs are configured on:

- Pin 9 – Phase A.
- Pin 10 – Phase B.
- Pin 11 – Phase C.

The PWM duty cycle is adjusted according to the selected motor speed.

### BEMF Interrupt Handling

The analog comparator interrupt detects the rising and falling transitions of the selected floating motor phase.

Based on the detected transition, the firmware advances the commutation sequence.

This allows the motor controller to synchronize its switching sequence with the rotor's electrical position once the motor reaches sufficient speed.

---

# Motor Speed Control

The motor speed is controlled by modifying the PWM duty cycle.

The firmware uses two push buttons connected to the Arduino UNO.

| Parameter | Configuration |
|---|---|
| Speed Up | A0 |
| Speed Down | A1 |
| Minimum PWM Duty | 50 |
| Maximum PWM Duty | 255 |
| PWM Resolution | 8-bit |
| Initial PWM Duty | 150 |

The initial PWM value and speed limits can be adjusted in the firmware according to the motor and power-stage characteristics.

---

# Hardware Development

The project was developed through two major hardware implementation stages.

## Stage 1: Perfboard Prototype

The initial motor controller was implemented on a perfboard.

The complete circuit, including the Arduino interface, gate driver ICs, MOSFET switching stages, and BEMF sensing network, was assembled and tested.

The purpose of this stage was to:

- Verify circuit connections.
- Test the gate driver operation.
- Validate MOSFET switching.
- Verify motor startup.
- Test BEMF-based commutation.
- Evaluate PWM speed control.

**Prototype Images:**

Add your actual perfboard photographs below.

<img width="1600" height="1204" alt="image" src="https://github.com/user-attachments/assets/9b738a71-3dc2-48f6-9f07-1f47367ac3cc" />


## Stage 2: PCB Design Using KiCad

After developing the working prototype, the motor driver circuit was designed as a dedicated PCB using KiCad.

The PCB design integrates the three half-bridge gate driver stages and the associated MOSFET power switching circuitry.

### PCB Design Process

1. Circuit schematic development.
2. Component selection.
3. Schematic symbol placement.
4. Electrical connection and net assignment.
5. PCB footprint assignment.
6. PCB component placement.
7. Track routing.
8. Design Rule Check (DRC).
9. PCB visualization and fabrication preparation.

### KiCad Schematic

Add your exported KiCad schematic here.

<img width="1120" height="787" alt="image" src="https://github.com/user-attachments/assets/6d811623-304b-49f9-b681-9ac247de7677" />


### PCB Layout

Add your PCB layout screenshot here.

<img width="813" height="777" alt="image" src="https://github.com/user-attachments/assets/f2361591-a9e6-4ecb-a945-b7808b27d98b" />


### 3D PCB Model

Add your KiCad 3D viewer screenshot here.

<img width="852" height="785" alt="image" src="https://github.com/user-attachments/assets/83c69caa-c867-42d7-b4ca-7cf8b7b23831" />


---

# Circuit Diagram

The motor controller consists of three identical half-bridge stages.

Each stage drives one motor phase.

The complete system includes:

- Arduino UNO control circuit.
- Three IR2104S gate driver circuits.
- Six MOSFET switching devices.
- BEMF sensing voltage divider network.
- Virtual neutral reference network.
- Speed control push buttons.
- DC power supply.

---

# Testing and Validation

The developed prototype is evaluated through the following testing procedures.

| Test | Objective |
|---|---|
| Power Supply Test | Verify the DC supply and gate driver supply |
| Gate Driver Test | Check high-side and low-side gate control |
| MOSFET Switching Test | Verify inverter switching operation |
| Motor Startup Test | Evaluate open-loop startup |
| BEMF Detection Test | Verify floating-phase zero-crossing detection |
| Commutation Test | Validate the six-step switching sequence |
| Speed Control Test | Verify speed increment and decrement |
| PCB Design Validation | Check schematic connectivity and PCB design rules |

### Expected Observations

- The motor should start using the initial open-loop commutation sequence.
- BEMF zero crossings should be detected after sufficient motor rotation.
- The controller should perform six-step commutation.
- Increasing PWM duty should increase motor speed within the operating range.
- Decreasing PWM duty should reduce motor speed.

Add your measured test results, oscilloscope waveforms, and motor performance observations here.

---

# Applications

- Drone propulsion systems.
- Small electric propulsion systems.
- Robotics.
- Electric vehicle research prototypes.
- Industrial motor control experiments.
- Embedded motor control applications.
- Educational power electronics projects.
- DIY electronic speed controllers.

---

# Future Improvements

The project can be further developed through the following improvements:

- Closed-loop RPM measurement and display.
- LCD-based real-time motor parameter monitoring.
- ESP32-based wireless monitoring.
- Blynk IoT dashboard integration.
- Motor temperature monitoring.
- Overcurrent protection.
- Undervoltage protection.
- MOSFET thermal protection.
- Improved startup reliability.
- Optimized PCB layout and thermal management.
- Improved motor speed regulation.
- Advanced motor control algorithms.

---

# Safety Considerations

- Verify all circuit connections before applying power.
- Use a current-limited DC supply during initial testing.
- Ensure adequate MOSFET and gate driver ratings.
- Provide sufficient PCB copper area and thermal management.
- Avoid short circuits between motor phases.
- Verify gate drive waveforms before connecting the motor.
- Use suitable protection against supply transients.
- Disconnect the power supply before modifying the circuit.

---

# Repository Structure

```text
Sensorless-BLDC-Motor-Drive/
│
├── README.md
│
├── Arduino_Code/
│   └── Sensorless_BLDC_Controller/
│       └── Sensorless_BLDC_Controller.ino
│
├── Circuit_Diagram/
│   ├── circuit_diagram.png
│   └── circuit_schematic.pdf
│
├── PCB_Design/
│   ├── KiCad_Project/
│   ├── Gerber_Files/
│   ├── pcb_layout.png
│   ├── pcb_3d.png
│   └── kicad_schematic.png
│
├── Hardware_Prototype/
│   ├── perfboard_top.jpg
│   ├── perfboard_side.jpg
│   └── motor_testing.jpg
│
├── Testing_Results/
│   ├── bemf_waveforms/
│   └── motor_testing/
│
└── Datasheets/
    ├── Arduino_UNO.pdf
    ├── IR2104S.pdf
    └── MOSFET_Datasheet.pdf
```

---

# Technologies Used

| Category | Technology |
|---|---|
| Microcontroller | Arduino UNO |
| Programming Language | Embedded C/C++ |
| Development Environment | Arduino IDE |
| Gate Driver | IR2104S |
| Power Electronics | N-Channel MOSFETs |
| PCB Design | KiCad |
| Motor Control Technique | Six-Step Commutation |
| Rotor Position Detection | BEMF Zero-Crossing Detection |
| Speed Control | PWM |

---

# License

This project is licensed under the MIT License.

See the [LICENSE](LICENSE) file for details.

---

# Author

**Kailash S**

Electrical and Electronics Engineering  
Chennai Institute of Technology

**Areas of Interest:**

- Embedded Systems
- Power Electronics
- BLDC Motor Control
- PCB Design
- Arduino and Microcontrollers
- IoT and Robotics
- Drone Technology

---

<p align="center">
  <b>Sensorless BLDC Motor Drive</b>
  <br>
  Designed and developed using Arduino UNO, power electronics, and KiCad.
  <br><br>
  ⭐ If you find this project useful, consider giving this repository a star!
</p>
