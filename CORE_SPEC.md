# CORE_SPEC.md — Internal Sync File
> **Purpose:** Single source of truth for pinouts, budget, and API endpoints.
> Upload this file to all Claude Projects to keep context synchronized.
> **Last Updated:** Phase 0 --- Architecture & Design
> **Repo:** https://github.com/rathgebtech/6Axis-AI-Robotic-Arm

---

## 🔷 System Identity

| Field | Value |
|---|---|
| **Project Name** | 6-Axis AI Robotic Arm |
| **Current Phase** | Phase 0 --- Architecture & Design |
| **Total Budget** | $1,000 USD |
| **Build Start** | TBD |
| **Primary Author** | rathgebtech |

---

## 🛠 Component Overview

| Role | Component | Model | Interface |
|---|---|---|---|
| Brain | Single Board Computer | Jetson Orin Nano 8GB | USB-C, GPMO, US Serial |
| Vision | Depth Camera | OAK-D Lite (DepthAI) | USB3 -> Jetson |
| Nervous System | Microcontroller | Teensy 4.1 | USB Serial <=> Jetson |
| Muscles (x6) | Closed-Loop Stepper | Servo42C (MKS) | UART <- TeeNSy |
| Power | PSU | TBD --- 24V / 15A target | Barrel Jack / Terminal |
| Frame | Mechanical | TBD --- Custom / 3D Print | N/A |

---

## 💠 Master BOM (Bill of Materials)

> **Status:** Placeholder --- Update with actual part numbers and vendor links in Phase 1.

| # | Component | Qty | Est. Unit Cost | Est. Total | Status |
|---|---|---|---|---|---|
| 1 | Jetson Orin Nano 8GB Dev Kit | 1 | $200 | $200 | ⚪ Not Ordered |
| 2 | OAK-D Lite (DepthAI) | 1 | $150 | $150 | ⚪ Not Ordered |
| 3 | Teensy 4.1 | 1 | $30 | $30 | ⚪ Not Ordered |
| 4 | Servo42C Closed-Loop Stepper | 6 | $45 | $270 | ⚪ Not Ordered |
| 5 | 24V Power Supply (15A) | 1 | $50 | $50 | ⚪ Not Ordered |
| 6 | NEMA 17 Stepper Motors | 6 | $15 | $90 | ⚪ Not Ordered |
| 7 | Frame Materials / 3D Filament | --- | $80 | $80 | ⚪ Not Ordered |
| 8 | Wiring, Connectors, Hardware | --- | $50 | $50 | ⚪ Not Ordered |
| 9 | Misc / Contingency (8%) | --- | --- | $80 | --- |
| | | | **TOTAL** | **~1,000** | |

---

## 🔊 Pinout Reference

> **Status:** Placeholders --- Populate with actual pins during Phase 1 hardware build.

### Jetson Orin Nano -> Teensy 4.1 (USB Serial)

| Signal | Jetson Pin | Wire | Teensy Pin | Notes |
|---|---|---|---|---|
| USB Serial TX | USB-A Port | USB Cable | USB (Native) | /dev/ttyACM0 |
| USB Serial RX | USB-A Port | USB Cable | USB (Native) | Bidirectional |
| Power (5V) | USB-A Port | USB Cable | VIN | Bus powered |

### Teensy 4.1 -> Servo42C (UART per Axis)

| Axis | Joint | Teensy TX Pin | Teensy RX Pin | Servo42C Addr | Notes |
|---|---|---|---|---|---|
| 1 | Base Rotation | `[TBD]` | `[TBD]` | 0x01 | UART1 |
| 2 | Shoulder | `[TBD]` | `[TBD]` | 0x02 | UART2 |
| 3 | Elbow | `[TBD]` | `[TBD]` | 0x03 | UART3 |
| 4 | Wrist Roll | `[TBD]` | `[TBD]` | 0x04 | UART4 |
| 5 | Wrist Pitch | `[TBD]` | `[TBD]` | 0x05 | UART5 |
| 6 | End Effector | `[TBD]` | `[TBD]` | 0x06 | UART6 |

### Jetson Orin Nano GPIO (Expansion --- TBD)

| Signal | GPIO Pin | Purpose | Notes |
|---|---|---|---|
| Emergency Stop | `[TBD]` | HW kill switch | Active LOW |
| Status LED | `[TBD]` | System heartbeat | `[TBD]` |
| `[TBD]` | `[TBD]` | `[TBD]` | --- |

---

## 🌀 API Endpoints & ROS2 Topics

> **Status:** Placeholder --- Implement during Phase 2 & 3.

### ROS2 Topics (Internal)

| Topic Name | Type | Publisher | Subscriber | Description |
|---|---|---|---|---|
| `/arm/joint_commands` | `JointTrajectory` | Vision AI node | Firmware bridge | Target joint angles |
| `/arm/joint_states` | `JointState` | Firmware bridge | Controller | Current position feedback |
| `/arm/spatial_target` | `PoseStamped` | DepthAI node | IK solver | 3D target from vision |
| `/arm/emergency_stop` | `Bool` | Any | All | Kill switch topic |
| `/arm/telemetry` | `Float32MultiArray` | Firmware bridge | Analytics node | Current, temp, error |

### Serial Protocol (Teensy <=> Jetson)

```
Command Packet Format (ASCII):
  CMD:<AXIS>:<POSITION>:<SPEED>:<CHECKSUM>\n
```

| Command | Format | Example | Response |
|---|---|---|---|
| Move Axis | `CMD:<ax>:<pos>:<spd>:<chk>` | `CMD:01:09000:050:XY` | `ACK:01:<pos>:OK` |
| Query Position | `QRY:<ax>` | `QYV:02` | `POS:02:<pos>` |
| Emergency Stop | `ESTOP` | `ESTOP` | `HALT:ALL` |
| Status Request | `STAT``| `STAT``| `STATUS:<json>` |

---

## 📁 Folder Sync Reference

| Folder | Contents | Claude Project Context |
|---|---|---|
| `/docs` | System maps, CAD renders, wiring diagrams | Architecture decisions |
| `/firmware` | Teensy C++ code, PlatformIO config | Embedded dev context |
| `/vision_ai` | DepthAI scripts, ROS2 nodes | Vision/AI dev context |
| `/analytics` | CSV logs, benchmark plots | Data analysis context |

---

## 🔀 Update Log

| Date | Phase | Change Summary | Updated By |
|---|---|---|---|
| 2025-05-09 | 0 | Initial CORE_SPEC created --- all placeholders | rathgebtech |
| `[TBD]` | 1 | Populate actual pinouts after hardware build | --- |
| `[TBD]` | 2 | Add serial protocol implementation details | --- |
| `[TBD]` | 3 | Add ROS2 topic implementations | --- |

---

> Sync Note: When uploading to a new Claude Project, prepend with:
> *This is my CORE_SPEC.md --- the ground truth for my 6-axis robotic arm project. Use this for all technical context.*
