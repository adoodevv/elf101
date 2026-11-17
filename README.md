# Elf-101

**A Christmas helper robot: an SO-101 arm with an RDK X5 as its brain.** Put wrapped gifts and ornaments on the table, say what you want ("give me the red gift", "put the ornaments in the basket"), and Elf-101 finds each one with the stereo camera, picks it up and delivers it.

Built for **Xmas Robots 2026** and the **Robotics Dream Keeper Challenge, Winter 2026** by D-Robotics. The RDK X5 board is sponsored by D-Robotics.

> Status: **Stage 2 (design)**. The board is on its way; Stage 1 bring-up starts when it arrives. Progress is tracked in [ROADMAP.md](ROADMAP.md).

## How RDK is used

| RDK X5 capability | What Elf-101 uses it for |
|---|---|
| BPU (10 TOPS) | Real-time object detection of gifts and ornaments (YOLO from the RDK Model Zoo) |
| Dual MIPI CSI / stereo camera | Depth for each detected object, so the arm knows where to reach without a hand-measured table height |
| 8x Cortex-A55 CPU | Grasp planning, inverse kinematics and servo control for the SO-101 over USB serial |
| ROS 2 (TogetheROS.Bot) | The camera, detector, planner and arm talk over ROS 2 topics and actions |
| GPIO | A physical emergency-stop button that cuts arm motion with no software in the way |

## Documents

- [Proposal](docs/PROPOSAL.md): scenario, user, measurable goals, what's new
- [Architecture](docs/ARCHITECTURE.md): data flow, modules, compute allocation, ROS 2 graph
- [Engineering plan](docs/ENGINEERING_PLAN.md): bill of materials, risks, repository layout
- [Roadmap](ROADMAP.md): stages and dates

## Hardware

- D-Robotics RDK X5 (sponsored) with the RDK stereo camera module
- SO-101 robot arm (follower) with Feetech STS3215 servos
- Momentary push button on a GPIO pin as an emergency stop
- Christmas props: small wrapped gift boxes, plastic ornaments, a basket and a small tree

## License

MIT. See [LICENSE](LICENSE).
