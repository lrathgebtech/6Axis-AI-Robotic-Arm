# /vision_ai — Jetson Orin + OAK-D Lite Pipeline

> **Status:** `⚪ Phase 3 — Pending`
> Vision AI development begins after serial control (Phase 2) is validated.

---

## Planned Structure

```
vision_ai/
└── oak_d_pipeline/
    ├── requirements.txt          ← Python dependencies
    ├── launch/
    │   └── spatial_detection.launch.py  ← ROS2 launch file
    └── src/
        ├── depth_node.py         ← DepthAI camera node
        ├── detection_node.py     ← Object detection pipeline
        ├── ik_bridge_node.py     ← Coord → joint angle bridge
        └── arm_controller.py    ← ROS2 → Serial command sender
```

## Tech Stack

| Tool | Purpose |
|---|---|
| DepthAI SDK | OAK-D Lite spatial inference |
| ROS2 Humble | Node communication framework |
| Python 3.10 | Pipeline scripting |
| OpenCV | Pre/post processing |
| NumPy | Coordinate math |

## Key ROS2 Topics

| Topic | Direction | Description |
|---|---|---|
| `/arm/spatial_target` | OUT | 3D XYZ target from DepthAI |
| `/arm/joint_commands` | OUT | Joint angles to firmware bridge |
| `/arm/joint_states` | IN | Current position feedback |
| `/arm/emergency_stop` | BOTH | Kill switch |

*← [Back to README](../README.md)*
