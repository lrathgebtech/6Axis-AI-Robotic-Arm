# /firmware — Teensy 4.1 Motion Control

> **Status:** `⚪ Phase 2 — Pending`
> Firmware development begins after hardware build (Phase 1) is complete.

---

## Planned Structure

```
firmware/
└── teensy_main/
    ├── platformio.ini        ← PlatformIO build config
    ├── src/
    │   ├── main.cpp          ← Entry point, serial handler
    │   ├── motion.cpp        ← Joint motion & IK interface
    │   ├── servo42c.cpp      ← Servo42C UART driver
    │   └── safety.cpp        ← Limits, estop, watchdog
    └── include/
        ├── config.h          ← Pin definitions, constants
        ├── motion.h
        └── servo42c.h
```

## Tech Stack

| Tool                      | Purpose                           |
|---------------------------|-----------------------------------|
| PlatformIO                | Build system (VS Code extension)  |
| C++17                     | Firmware language                 |
| Servo42C UART Protocol    | Stepper motor control             |
| USB Serial (native)       | Jetson ↔ Teensy bridge            |

## Serial Protocol Preview

```cpp
// Command format (ASCII over USB Serial)
// CMD:<AXIS>:<POSITION>:<SPEED>:<CHECKSUM>\n
// Example: CMD:01:09000:050:XY\n

// Response format
// ACK:<AXIS>:<ACTUAL_POS>:<STATUS>\n
```

*← [Back to README](../README.md)*
