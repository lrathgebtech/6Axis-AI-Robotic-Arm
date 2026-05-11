<div align="center">

# [CODENAME] 🦾
### A 6-Axis AI-Driven Robotic Arm — Designed, Built, and Programmed From Scratch

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-In_Development-orange)](ROADMAP.md)
[![Brain](https://img.shields.io/badge/Brain-Jetson_Orin_Nano-76B900)]()
[![MCU](https://img.shields.io/badge/MCU-Teensy_4.1-DC322F)]()
[![YouTube](https://img.shields.io/badge/YouTube-Build_Series-FF0000)]()

**[ Architecture ](#-system-architecture)** · **[ Hardware ](#-hardware-stack)** · **[ Software ](#-software-stack)** · **[ Repo Map ](#-repository-structure)** · **[ Roadmap ](#-roadmap)** · **[ Tests ](#-testing--benchmarks)**

</div>

---

## 🎯 Project Goal

> _[ONE-LINE TAGLINE — e.g., "A voice-controlled robotic arm that sees, thinks, and reacts — every gear, board, and line of code designed in-house."]_

This repository documents the end-to-end engineering of a **6-axis robotic arm** with embedded **Vision-Language Model (VLM)** intelligence — designed entirely solo. The arm understands voice commands, perceives its environment with stereo vision, and responds through both motion and visual cues.

> 📽️ _Demo GIF coming soon — see [Roadmap](#-roadmap) for milestone targets._

---

## 🧠 System Architecture

The system follows a biologically inspired three-tier model: **Brain → Nervous System → Muscles.**

```
                        🎤 USER VOICE INPUT
                                │
                                ▼
        ┌───────────────────────────────────────────────┐
        │        JETSON ORIN NANO   ░ BRAIN ░           │
        │  • Vision-Language Model (VLM)                │
        │  • Speech-to-Text / Text-to-Speech            │
        │  • Motion planning & inverse kinematics       │
        │  • Depth perception via OAK-D Lite            │
        └──────────┬─────────────────────────┬──────────┘
                   │ USB Serial              │ Data
                   ▼                         ▼
        ┌──────────────────────┐    ┌───────────────────┐
        │  TEENSY 4.1          │    │ WS2812B LED Ring  │
        │  ░ NERVOUS SYSTEM ░  │    │ Speaker           │
        │  • Real-time control │    │ (Emotion feedback)│
        │  • Sensor fusion     │    └───────────────────┘
        │  • Safety interlocks │
        └────┬──────────┬──────┘
             ▼          ▼
   ┌──────────────┐ ┌─────────────┐
   │ SERVO42C × 3 │ │ FEETECH × 3 │ + GRIPPER MOTOR
   │ (Big joints) │ │ (Wrist)     │
   └──────────────┘ └─────────────┘
                ░ MUSCLES ░
```

Full details: [`docs/architecture/system-overview.md`](docs/architecture/system-overview.md)

---

## Hardware Stack

| Layer | Component | Role | Custom-Designed |
|---|---|---|:---:|
| **Brain** | Jetson Orin Nano | AI inference, planning | — |
| **Vision** | OAK-D Lite | Stereo depth + RGB | — |
| **MCU** | Teensy 4.1 | Real-time control | — |
| **Big-Joint Drives** | Servo42C × 3 | Base / shoulder / elbow | — |
| **Wrist Drives** | Feetech serial servos × 3 | Wrist roll / pitch / yaw | — |
| **Gripper** | DC motor (TBD) | End-effector | ✅ |
| **Reduction** | Cycloidal drives | Joint torque amplification | ✅ |
| **Cable Drive** | Capstan drive | Wrist actuation | ✅ |
| **Collision Sensing** | Proximity sensors | Pre-impact stop | ✅ PCB |
| **Audio I/O** | Microphone + speaker | Voice interaction | — |
| **Feedback** | WS2812B LED ring | "Emotional" visual cues | ✅ PCB |
| **Power** | Custom PSU board | Multi-rail distribution | ✅ |
| **Structure** | 3D-printed + machined | Arm links & housings | ✅ |

---

## Software Stack

| Layer | Stack |
|---|---|
| **High-Level (Jetson)** | Python · PyTorch · DepthAI SDK · Whisper / TTS · ROS 2 *(planned)* |
| **VLM** | Local Vision-Language Model on Jetson Orin Nano *(model TBD)* |
| **Mid-Level (Comms)** | USB Serial · Custom binary protocol |
| **Low-Level (Teensy)** | C++ · PlatformIO · Custom servo driver library |
| **CAD** | Fusion 360 (mechanical) · KiCad (electronics) |
| **Simulation** | MuJoCo or Gazebo *(planned, for IK validation)* |

---

## Repository Structure

```
6Axis-AI-Robotic-Arm/
├── 📄 README.md
├── 📄 LICENSE                       # Apache 2.0
├── 📄 ROADMAP.md                    # Project phases & milestones
├── 📄 CHANGELOG.md
│
├── 📂 docs/                         # Long-form documentation
│   ├── architecture/                # System overviews + diagrams
│   ├── design-decisions/            # ADRs — why X over Y
│   └── safety.md                    # Safety design & E-stop logic
│
├── 📂 hardware/                     # 🔌 [Hardware & Electronics project]
│   ├── mechanical/
│   │   ├── cad/                     # Fusion 360, STEP source files
│   │   ├── stl/                     # 3D-printable parts
│   │   ├── drawings/                # PDF technical drawings
│   │   ├── cycloidal-drives/
│   │   ├── capstan-drive/
│   │   └── gripper/
│   ├── electronics/
│   │   ├── pcbs/                    # KiCad source files
│   │   ├── schematics/
│   │   └── wiring-diagrams/
│   └── BOM/
│       └── master-bom.csv           # Single source of truth
│
├── 📂 firmware/                     # ⚙️ [Firmware Low-Level project]
│   ├── teensy-main/                 # Main Teensy 4.1 firmware
│   ├── servo-drivers/               # Servo42C + Feetech protocols
│   ├── sensor-drivers/              # IMU, proximity, etc.
│   └── tests/                       # Unit tests
│
├── 📂 vision_ai/                    # 🧠 [Vision & AI High-Level project]
│   ├── perception/                  # OAK-D Lite stereo + depth
│   ├── vlm/                         # Vision-Language Model integration
│   ├── voice/                       # STT input + TTS output
│   ├── motion-planning/             # IK, trajectories
│   ├── led-feedback/                # WS2812B "expressions"
│   └── jetson-config/               # Orin Nano setup scripts
│
├── 📂 tests/                        # 🧪 [Data Lab project]
│   ├── benchmarks/
│   │   ├── mechanical/              # Repeatability, payload, backlash
│   │   ├── electrical/              # Power draw, thermal
│   │   ├── ai-performance/          # Inference latency, accuracy
│   │   └── safety/                  # E-stop response time
│   ├── protocols/                   # How to run each test
│   └── results/                     # CSV data + plots
│
├── 📂 media/                        # 📷 [Tech PM project]
│   ├── photos/
│   ├── renders/
│   └── youtube/                     # Thumbnails, b-roll
│
└── 📂 project-management/           # 📋 [Tech PM project]
    ├── gantt-timeline.md
    └── vlog-scripts/
```

---

## Roadmap

| Phase | Focus | Progress |
|:---:|---|---|
| **0** | Architecture & spec lock-in | `████░░░░░░` 40% |
| **1** | Mechanical design (CAD · cycloidal · capstan) | `░░░░░░░░░░`  0% |
| **2** | Electronics (PCBs · power · sensing) | `░░░░░░░░░░`  0% |
| **3** | Firmware (Teensy · servo control · safety) | `░░░░░░░░░░`  0% |
| **4** | High-level AI (Jetson · OAK-D · VLM) | `░░░░░░░░░░`  0% |
| **5** | Voice + LED feedback integration | `░░░░░░░░░░`  0% |
| **6** | Full autonomous voice-controlled demo | `░░░░░░░░░░`  0% |

Detailed milestones: [`ROADMAP.md`](ROADMAP.md)

---

## Testing & Benchmarks

Every subsystem is validated against measurable targets — so each iteration shows *why* it's better than the last. This is the core of the **Data Lab** workflow.

| Test Category | Example Metrics |
|---|---|
| **Mechanical** | Repeatability (mm) · max payload (g) · joint backlash (°) |
| **Electrical** | Power draw per joint (W) · thermal stability over 30 min |
| **AI** | VLM inference latency (ms) · voice command accuracy (%) |
| **Safety** | E-stop response time (ms) · collision-stop distance (mm) |

All raw data + analysis: [`tests/results/`](tests/)

---

## Build Series on YouTube

Every major milestone is documented as a vlog episode aimed at hobby engineers — full transparency including the failures.

| Ep. | Topic | Status |
|:---:|---|:---:|
| 01 | Project kickoff + architecture overview | ⚪ |
| 02 | Designing the cycloidal drives | ⚪ |
| 03 | First PCB design pass | ⚪ |
| ... | | |

_YouTube link coming soon_ · 📜 Scripts in [`project-management/vlog-scripts/`](project-management/vlog-scripts/)

---

## Why This Repo Exists

This repository serves **two audiences**:

| Audience | Where to Start |
|---|---|
| 👔 **Recruiters** | [`docs/architecture/`](docs/architecture/) → [`tests/results/`](tests/) → repo navigation |
| 🛠️ **Hobby engineers** | [YouTube series](#-build-series-on-youtube) → [`hardware/BOM/`](hardware/BOM/) → [`firmware/`](firmware/) |

Everything here — every design decision, test result, and line of code — is meant to demonstrate **end-to-end engineering capability**, from cycloidal gear math to VLM prompt design.

---

## 📜 License

Licensed under the **Apache License 2.0** — see [LICENSE](LICENSE) for full terms.

You are free to use, modify, and distribute this work, **including commercially**, provided you preserve attribution and the included patent grant.

---

## Contact

Built by **Lukas Rathgeb** · [LinkedIn](www.linkedin.com/in/lukasrathgeb) · [YouTube](https://youtube.com/@rathgebtech?si=JU9IMZOkAiA6r8uq) · [Email](mailto:lrathgeb.tech@gmail.com)

> _"The best way to learn how something works is to build it twice."_