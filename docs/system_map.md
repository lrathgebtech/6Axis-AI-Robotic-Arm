# SYSTEM_MAP.md — Detailed Architecture & Signal Flow
> Part of: [6-Axis AI Robotic Arm](../README.md)
> See [`CORE_SPEC.md`](../CORE_SPEC.md) for pinouts and BOM.

---

## Signal Flow Diagram

```
┌────────────────────────────────────────────────────────────────┐
│                     PERCEPTION LAYER                           │
│                                                                │
│   ┌──────────────┐   USB 3.0      ┌─────────────────────────┐  │
│   │  OAK-D Lite  │───────────────▶│    Jetson Orin Nano     │  │
│   │  (DepthAI)   │                │         8GB             │  │
│   │              │   RGB + Depth  │                         │  │
│   │ • RGB 1080p  │   @ 30fps      │  ┌───────────────────┐  │  │
│   │ • Stereo IR  │                │  │  DepthAI Runtime  │  │  │
│   │ • IMU        │                │  │  • Object Detect  │  │  │
│   └──────────────┘                │  │  • Depth Estim.   │  │  │
│                                   │  │  • 3D Coordinate  │  │  │
│                                   │  └────────┬──────────┘  │  │
│                                   │           │ ROS2 Topic  │  │
│                                   │  ┌────────▼──────────┐  │  │
│                                   │  │   IK Solver Node  │  │  │
│                                   │  │  (Python / ROS2)  │  │  │
│                                   │  └────────┬──────────┘  │  │
│                                   └──────────┼──────────────┘  │
└───────────────────────────────────────────────┼────────────────┘
                                                │ USB Serial
                                                │ /dev/ttyACM0
┌───────────────────────────────────────────────▼────────────────┐
│                     CONTROL LAYER                              │
│                                                                │
│                   ┌─────────────────┐                          │
│                   │   Teensy 4.1    │                          │
│                   │  (C++ Firmware) │                          │
│                   │                 │                          │
│                   │ • Serial Parser │                          │
│                   │ • Motion Queue  │                          │
│                   │ • Safety Limits │                          │
│                   │ • PID / Timing  │                          │
│                   └──┬──┬──┬──┬──┬─┘                           │
│              UART1   │  │  │  │  │ UART6                       │
└─────────────────────-┼--┼--┼--┼--┼─────────────────────────────┘
                       │  │  │  │  │
┌──────────────────────▼──▼──▼──▼──▼─────────────────────────────┐
│                   ACTUATION LAYER                              │
│                                                                │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐                        │
│  │Servo42C  │ │Servo42C  │ │Servo42C  │  ...×6 total           │
│  │  Axis 1  │ │  Axis 2  │ │  Axis 3  │                        │
│  │  (Base)  │ │(Shoulder)│ │ (Elbow)  │                        │
│  └──────────┘ └──────────┘ └──────────┘                        │
│       │             │             │    Encoder Feedback Loop   │
│       └─────────────┴─────────────┘──────────────────────────▶ │
│                    NEMA 17 Steppers                            │
└────────────────────────────────────────────────────────────────┘
```

---

## Layer Responsibilities

| Layer               | Hardware            | Software        | Latency Budget|
|---------------------|---------------------|-----------------|---------------|
| **Perception**      | OAK-D Lite          | DepthAI, Python | ~33ms (30fps) |
| **Planning**        | Jetson Orin         | ROS2, IK Solver | ~10ms         |
| **Serial Bridge**   | USB (Jetson↔Teensy) | pyserial / UART | ~2ms          |
| **Motion Control**  | Teensy 4.1          | C++ Firmware    | ~1ms          |
| **Actuation**       | Servo42C × 6        | UART commands   | ~5ms per axis |
| **Total Target**    | End-to-end          | Full pipeline   | **< 60ms**    |  

---

## Coordinate System

```
         Z (Up)
         │
         │
         │
         └──────── X (Forward / Reach)
        /
       /
      Y (Lateral)

  Joint 0: Base — Rotates about Z axis
  Joint 1: Shoulder — Rotates about Y axis
  Joint 2: Elbow — Rotates about Y axis
  Joint 3: Wrist Roll — Rotates about X axis
  Joint 4: Wrist Pitch — Rotates about Y axis
  Joint 5: End Effector — Rotates about Z axis
```

---

## Phase Gate: What Unlocks What

```
Phase 0 ──▶ Phase 1 ──▶ Phase 2 ──▶ Phase 3 ──▶ Phase 4
Design       Hardware     Serial        Vision       Full Demo
             ✓ Frame      ✓ Teensy      ✓ DepthAI    ✓ Benchmarks
             ✓ Wiring     ✓ IK Solver   ✓ ROS2       ✓ Vlog
             ✓ Power      ✓ Safety      ✓ Spatial Nav ✓ Portfolio
```

---

## CAD Renders

> *Place rendered images and wiring diagrams in this folder (`/docs/renders/`).*
> *File naming convention: `phase{N}_{description}.png`*
> Example: `phase1_wiring_diagram.png`, `phase1_frame_assembly.png`

---

*← Back to [README](../README.md) | [CORE_SPEC](../CORE_SPEC.md) →*
