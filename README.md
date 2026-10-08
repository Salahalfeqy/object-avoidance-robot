# Autonomous Obstacle Avoiding Robot Car 🚗🤖

> An Arduino-powered 4WD smart robot car that autonomously navigates physical environments, detects obstacles using ultrasonic sensing, and dynamically calculates safe avoidance maneuvers.

---

## Badges
![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Arduino%20UNO-00979C?style=flat-square&logo=arduino)
![C++](https://img.shields.io/badge/Language-C%2B%2B%20%2F%20Wiring-00599C?style=flat-square&logo=c%2B%2B)
![Hardware](https://img.shields.io/badge/Hardware-L298N%20%7C%20HC--SR04%20%7C%20SG90-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## Hardware Gallery
| Bottom View (Motors & Chassis) | Isometric View (Electronics & Sensor Mount) |
| :---: | :---: |
| <img width="300" alt="Bottom View" src="https://github.com/user-attachments/assets/592ef4c1-5efd-4288-bfa4-baef89e7eebe" /> | <img width="300" alt="Isometric View" src="https://github.com/user-attachments/assets/37361220-9be2-42cd-b246-faebbf84aeaf" /> |

> **Note:** Save the project images inside a `docs/` folder in your repository with the filenames `robot-bottom.jpg` and `robot-iso.jpg`.

---

## Tech Stack & Hardware Components

### Software & Firmware
- **Language:** C++ / Arduino Wiring
- **Libraries:**
  - `<Servo.h>`: Controls the angular position of the micro servo motors.
  - `<NewPing.h>`: Provides high-accuracy timing and distance calculations for the ultrasonic sensor.

### Hardware Bill of Materials (BOM)
- **Microcontroller:** Arduino UNO R3 (ATmega328P)
- **Motor Driver:** L298N Dual H-Bridge Motor Driver Module
- **Chassis:** 4WD Smart Robot Acrylic Chassis Kit
- **Drive Motors:** 4 × TT DC Gear Motors with matching wheels
- **Sensor:** HC-SR04 Ultrasonic Distance Sensor
- **Actuators:** 2 × TowerPro SG90 Micro Servo Motors
- **Power Supply:** External multi-cell battery holder (for motor driver and logic supply)
- **Prototyping:** Mini breadboard, standoffs, and male-to-male / male-to-female jumper wires

---

## Project Overview & Features

### Overview
This project implements an autonomous mobile robotic vehicle driven by an Arduino UNO. The robot navigates autonomously in forward drive until its path is blocked. When an obstacle is detected within a predefined threshold (20 cm), the robot halts, shakes/wiggles to ensure mechanical clearance, reverses briefly, pans its ultrasonic sensor to the left and right, and commits to the path offering the highest clearance.

### Key Features
- **Real-Time Obstacle Detection:** Continual distance sampling powered by the `NewPing` library.
- **Dynamic Directional Scanning:** Servo-mounted ultrasonic head scans both the left (`170°`) and right (`50°`) sectors when blocked.
- **Autonomous Navigation & Recovery:** Automatic reversing and pivot turning toward the clearest direction.
- **Fail-Safe Thresholding:** Zero-reading handling (`ping_cm == 0`) mapped to clear space to prevent sensor freeze states.

---

## Circuit Schematic & Pin Mapping

### 1. Circuit Architecture (Mermaid)

```mermaid
graph TD
    subgraph Power["Power Supply"]
        BAT[Battery Pack]
    end

    subgraph Controller["Arduino UNO R3"]
        A1[Pin A1 - Trig]
        A2[Pin A2 - Echo]
        D4[Pin 4 - Right Fwd]
        D5[Pin 5 - Right Bwd]
        D6[Pin 6 - Left Bwd]
        D7[Pin 7 - Left Fwd]
        D10[Pin 10 - Scan Servo]
        D13[Pin 13 - Auxiliary Servo]
        GND_ARD[GND]
        VCC5[5V Output]
    end

    subgraph Sensor["HC-SR04 Ultrasonic Sensor"]
        US_VCC[VCC]
        US_TRIG[Trig]
        US_ECHO[Echo]
        US_GND[GND]
    end

    subgraph Servos["Actuators (Servos)"]
        SRV1[Scan Servo - D10]
        SRV2[Auxiliary Servo - D13]
    end

    subgraph MotorDriver["L298N Dual H-Bridge Driver"]
        IN1[IN1 - Right Fwd]
        IN2[IN2 - Right Bwd]
        IN3[IN3 - Left Bwd]
        IN4[IN4 - Left Fwd]
        PWR_IN[12V / Battery (+)]
        M_GND[GND (Common)]
        OUT12[OUT 1 & 2 -> Right Motors]
        OUT34[OUT 3 & 4 -> Left Motors]
    end

    %% Wiring Connections
    BAT --> PWR_IN
    BAT --> GND_ARD
    M_GND --> GND_ARD

    A1 --> US_TRIG
    A2 --> US_ECHO
    VCC5 --> US_VCC
    GND_ARD --> US_GND

    D10 --> SRV1
    D13 --> SRV2

    D4 --> IN1
    D5 --> IN2
    D6 --> IN3
    D7 --> IN4

    IN1 & IN2 --> OUT12
    IN3 & IN4 --> OUT34
