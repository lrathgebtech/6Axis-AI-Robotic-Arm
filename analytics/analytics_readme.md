# /analytics — Data Lab: Benchmarks & Telemetry

> **Status:** `⚪ Phase 2 — Logging begins when serial comms are live`

---

## Planned Structure

```
analytics/
├── logs/
│   ├── phase2_serial_latency.csv      ← Raw serial round-trip logs
│   ├── phase3_vision_latency.csv      ← End-to-end vision pipeline logs
│   └── phase4_accuracy_trials.csv     ← Positional accuracy test data
└── plots/
    ├── latency_comparison.png         ← Manual vs AI latency chart
    ├── accuracy_scatter.png           ← XYZ accuracy scatter plot
    └── efficiency_bar.png             ← Task completion time comparison
```

## CSV Log Schema (Planned)

### `serial_latency.csv`
```
timestamp, axis, command_sent_ms, ack_received_ms, round_trip_ms, position_target, position_actual, error_mm
```

### `accuracy_trials.csv`
```
trial_id, mode (manual|ai), target_x, target_y, target_z, actual_x, actual_y, actual_z, error_mm, completion_time_ms
```

## Benchmark Goals

| Metric                        | Target  | Stretch Goal |
|-------------------------------|---------|--------------|
| Serial round-trip             | < 5ms   | < 2ms        |
| End-to-end (vision→motion)    | < 100ms | < 60ms       |
| Positional accuracy           | ±2mm    | ±1mm         |
| Repeatability (10 trials)     | ±1.5mm  | ±0.5mm       |

*← [Back to README](../README.md)*
