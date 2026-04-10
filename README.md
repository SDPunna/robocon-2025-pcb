# Robocon 2025 - Multi-ESP Communication & Drive PCB

<div align="center">

![Team RAW 2025](https://img.shields.io/badge/Team-RAW%202025-red?style=for-the-badge)
![Altium Designer](https://img.shields.io/badge/Altium-Designer%2024.6.1-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

**A custom PCB design for coordinated mechanism control in robotics competition**

[Features](#features) • [Architecture](#architecture) • [Technical Details](#technical-details) • [Getting Started](#getting-started)

</div>

---

## 📋 Overview

This repository contains the complete PCB design for our Robocon 2025 robot's control system. The board serves as the central nervous system, coordinating multiple mechanisms through a master-slave ESP32 architecture with dedicated motor driver integration.

**Team Members:** Vaishnavi and Shreehari

**Institution:** St. Francis Institute of Technology

## ✨ Features

- **Multi-ESP Architecture**: Master-slave configuration for distributed processing
- **I2C Communication Network**: Efficient inter-microcontroller communication
- **Motor Drive Integration**: MDDS 10 motor drivers with UART/Serial control
- **PS Controller Support**: Bluetooth interface via ESP32-U
- **Custom Component Libraries**: ESP32-U and ESP32-S3 footprints created from scratch
- **Professional PCB Layout**: 2-layer design optimized for power and signal integrity

## 🏗️ System Architecture

### Hardware Configuration

```
┌─────────────────────────────────────────────────────────────┐
│                    R2_Circuit PCB                           │
│                                                             │
│  ┌──────────────┐      I2C Bus        ┌─────────────────┐  │
│  │  ESP32-U     │◄────────────────────►│   ESP32-S3      │  │
│  │  (Master)    │                      │   (Locomotion)  │  │
│  │              │                      │                 │  │
│  │ - Bluetooth  │                      │ - Motor Control │  │
│  │ - PS Control │                      │ - High Compute  │  │
│  └──────┬───────┘                      └────────┬────────┘  │
│         │                                       │           │
│         │ UART/Serial                          │ Motor     │
│         ▼                                       ▼ Outputs   │
│  ┌──────────────┐                      ┌─────────────────┐  │
│  │ MDDS 10      │                      │   ESP32-S3      │  │
│  │ Motor Driver │                      │   (Shooter)     │  │
│  │ (Shooter)    │                      │                 │  │
│  └──────────────┘                      └─────────────────┘  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### ESP32 Roles

| Module | Type | Primary Function | Communication |
|--------|------|------------------|---------------|
| **ESP32-U (Master)** | Main Controller | PS4/PS5 controller interface via Bluetooth, system coordination | I2C Master, UART TX |
| **ESP32-S3 (Locomotion)** | Slave | High-performance motor control for robot locomotion | I2C Slave |
| **ESP32-S3 (Shooter)** | Slave | Intake/shooter mechanism control | I2C Slave |

## 🔧 Technical Details

### Schematic Design Highlights

#### 1. **Net Label Methodology**
- **Advantage**: Net labels eliminate visual clutter from direct wire connections
- **Benefit**: Simplifies complex multi-point connections (I2C, power rails)
- **Result**: Clean, readable schematics that are easy to debug and modify
- **Example**: Power nets (+5V, GND), I2C signals (SDA, SCL), UART pins all use net labels for clarity

#### 2. **Modular Section Architecture**
The schematic is divided into functional blocks:
- **Master Section**: ESP32-U with PS controller interface
- **Locomotion Section**: ESP32-S3 with stepper/BLDC motor control
- **Shooter Section**: ESP32-S3 with intake mechanism drivers

This separation provides:
- Clear signal flow visualization
- Easier troubleshooting
- Simplified collaborative design

#### 3. **Strategic ESP Selection**
- **ESP32-U**: 
  - Chosen for integrated Bluetooth capabilities
  - Handles real-time PS controller input
  - Lower power consumption for control tasks
  
- **ESP32-S3**: 
  - Dual-core architecture for high computational loads
  - Faster processing for motor control algorithms
  - Dedicated hardware for PWM generation

### PCB Layout Highlights

#### 1. **Strategic Layer Stack-Up (2-Layer Design)**

```
┌─────────────────────────────────────────┐
│  TOP LAYER (Red)                        │
│  • Power Distribution (+5V, Motor Power)│
│  • Wide traces (50 mils)                │
│  • Minimal crossings                    │
└─────────────────────────────────────────┘
┌─────────────────────────────────────────┐
│  BOTTOM LAYER (Blue)                    │
│  • Signal routing (I2C, UART, GPIO)     │
│  • Thin traces (15 mils)                │
│  • High-speed data paths                │
└─────────────────────────────────────────┘
```

**Why this approach?**
- Power lines on top layer ensure:
  - Low impedance paths
  - Reduced voltage drop
  - Better thermal dissipation
- Signal lines on bottom provide:
  - Shorter routes
  - Controlled impedance
  - Minimal EMI from power switching

#### 2. **Trace Width Design Philosophy**

| Trace Type | Width | Rationale |
|------------|-------|-----------|
| **Power Lines** | 50 mils | High current capacity (motor drivers draw significant current), reduced resistive losses, improved thermal performance |
| **Signal Lines** | 15 mils | Adequate for digital signals, minimizes board space, maintains signal integrity for I2C (400kHz) and UART |

**Current Capacity**: 50 mil traces can safely handle 2-3A on 1oz copper, suitable for motor driver power requirements.

#### 3. **Ground Polygon Implementation**

- **Dual-layer ground pour**: Applied to both top and bottom layers
- **Benefits**:
  - **Noise Immunity**: Ground plane acts as shield against EMI
  - **Return Path**: Provides low-impedance return for high-frequency signals
  - **Thermal Management**: Distributes heat from motor drivers and ESP modules
  - **Reduced Crosstalk**: Separates signal traces with ground reference

#### 4. **Via Strategy**

- **Purpose**: Connecting low-power device grounds to microcontroller ground
- **Implementation**: Strategic via placement near ESP power pins
- **Advantage**: 
  - Ensures single-point grounding for sensitive digital circuits
  - Minimizes ground loops
  - Provides solid reference plane for both layers

### Communication Protocols

#### I2C Network
- **Clock Speed**: 100kHz (Standard Mode) / 400kHz (Fast Mode)
- **Topology**: Multi-slave configuration
- **Addressing**: Unique 7-bit addresses for each ESP32-S3 slave
- **Pull-ups**: 4.7kΩ resistors on SDA/SCL lines

#### UART/Serial to MDDS 10
- **Baud Rate**: 9600 bps (Simplified Serial Mode)
- **Frame Format**: 8N1 (8 data bits, no parity, 1 stop bit)
- **Commands**: Speed and direction control packets
- **Connection**: TX from ESP32-U → RX on motor driver


## 🚀 Getting Started

### Prerequisites

- **Altium Designer 24.6.1** or later
- **ESP-IDF** or **Arduino IDE** for firmware development
- **Git** for version control

### Opening the Project

1. Clone the repository:
```bash
git clone https://github.com/yourusername/robocon-2025-pcb.git
cd robocon-2025-pcb
```

2. Open the project:
   - Navigate to `Hardware/Altium_Project/`
   - Open `R2_Circuit.PrjPcb` in Altium Designer

3. Custom libraries:
   - Libraries are stored in `Hardware/Altium_Project/Libs/`
   - Altium will automatically load them when opening the project

### Manufacturing

1. Generate production files:
   - Use the included Output Job file
   - Generates Gerbers, NC Drill, BOM, and assembly files

2. Recommended PCB specifications:
   - **Layers**: 2
   - **Material**: FR-4
   - **Thickness**: 1.6mm
   - **Copper**: 1 oz (35µm)
   - **Solder Mask**: Green
   - **Silkscreen**: White
   - **Surface Finish**: HASL or ENIG

## 🛠️ Custom Libraries

We created custom schematic and PCB footprints for:

- **ESP32-U**: Master controller with Bluetooth
- **ESP32-S3**: High-performance slave controllers

These libraries include:
- Accurate pin mappings
- Verified footprints
- 3D models (where available)
- Best-practice design rules

Libraries are available in `Hardware/Altium_Project/Libs/` and can be reused in other projects.

## 📊 Bill of Materials (Key Components)

| Component | Quantity | Part Number | Description |
|-----------|----------|-------------|-------------|
| ESP32-U | 1 | ESP32-WROOM-32U | Master controller with Bluetooth |
| ESP32-S3 | 2 | ESP32-S3-WROOM-1 | High-performance slaves |
| MDDS 10 | 1 | Cytron MDDS10 | Dual channel motor driver |
| Stepper Driver | 1 | A4988/DRV8825 | Stepper motor control |
| Voltage Regulator | 3 | LM2596 | 5V buck converters |
| Connectors | Various | JST-XH, Molex | Power and signal headers |

## 🔍 Design Decisions & Lessons Learned

### Why I2C for Inter-ESP Communication?
- **Multi-drop capability**: Single bus for multiple slaves
- **Pin efficiency**: Only 2 wires (SDA, SCL) needed
- **Sufficient speed**: 400kHz adequate for control commands
- **Built-in addressing**: No additional multiplexing required

### Why 2-Layer PCB?
- **Cost-effective**: Significantly cheaper than 4-layer
- **Sufficient for our application**: No high-speed differential pairs
- **Easier to manufacture**: Faster turnaround time
- **Adequate grounding**: Polygon pours provide noise immunity

### Power Distribution Strategy
Placing power on the top layer was crucial because:
1. Motor drivers draw high current (up to 10A peak)
2. Shorter, wider traces reduce voltage drop
3. Better thermal dissipation through top layer copper
4. Easier visual inspection and probing

## 🧪 Testing & Validation

- [ ] Power supply verification (voltage levels, current draw)
- [ ] I2C communication test (address scanning, data integrity)
- [ ] UART communication to motor drivers
- [ ] PS controller pairing and input mapping
- [ ] Motor control functionality
- [ ] Thermal testing under load

## 📝 License

This project is open-source hardware. Feel free to use, modify, and distribute.

**Suggested License**: [CERN Open Hardware Licence Version 2 - Permissive](https://ohwr.org/cern_ohl_p_v2.txt)

## 🤝 Contributing

We welcome contributions! If you find issues or have improvements:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Submit a pull request

## 👥 Team

**Team RAW 2025**
- **Vaishnavi** - Hardware Design & PCB Layout
- **Shreehari** - Hardware Design & PCB Layout

**Institution**: St. Francis Institute of Technology

## 📧 Contact

For questions or collaboration opportunities, reach out through GitHub issues or contact the team.

---

<div align="center">

**Built with ⚡ for Robocon 2025**

*Showcasing engineering excellence through custom PCB design*

</div>
