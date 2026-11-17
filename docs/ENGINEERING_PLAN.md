# Engineering plan

## Bill of materials

| Item | Qty | Source | Interface / power | Cost |
|---|---|---|---|---|
| RDK X5 dev kit | 1 | Sponsored by D-Robotics | 5 V USB-C power | Sponsored |
| RDK stereo camera module | 1 | Sponsored by D-Robotics (with challenge registration) | MIPI CSI | Sponsored |
| SO-101 follower arm (6× Feetech STS3215) | 1 | Already owned | Servo bus board over USB serial; 7.4 V or 12 V servo supply | Owned |
| Servo power supply | 1 | Already owned | Matches the servo version | Owned |
| microSD card, 32 GB+ | 1 | Local | | ~USD 8 |
| Momentary push button + 2 jumper wires | 1 | Local | GPIO input with pull-up | ~USD 2 |
| Camera mount (3D printed) | 1 | Printed at Mind2Matter | Holds the stereo camera above the table | ~USD 3 filament |
| Props: small wrapped boxes, plastic ornaments, basket, mini tree | set | Local | | ~USD 10 |

## Roadmap

| Dates | Milestone |
|---|---|
| Nov 17–26 | Stage 2: proposal, architecture and plan published; camera mount designed |
| Board arrival + 3 days | Stage 1: flash, SSH, camera working, YOLO demo running on the BPU |
| +4 days | Arm driver on the X5: SO-101 moves from the board, stop button wired |
| +4 days | Detection + depth → 3D points; camera-to-arm calibration |
| +4 days | Full pick-and-place loop; 20-trial evaluation |
| By Dec 13 | Demo video (3–7 min), benchmark table, final docs |
| Dec 15 | Stage 3 submission; Xmas Robots 30 s video by Dec 20 |

The deadline is fixed, so the board's arrival date decides how much of the stretch goals (voice input, hanging ornaments on the tree) gets built.

## Top risks

| # | Risk | Trigger | Mitigation | Fallback |
|---|---|---|---|---|
| 1 | Board arrives late (shipping + customs to Ghana) | Not delivered by Nov 30 | Finish all design and arm code beforehand; test the arm code on a laptop | Reduce scope to one task (pick by colour, place in basket) |
| 2 | SO-101 servo driver doesn't run on the X5 (aarch64) | Install fails | Use the Feetech serial protocol directly in Python, which only needs pyserial | Keep the arm on the laptop, but this breaks "all on board", so it's a last resort |
| 3 | Stereo depth too noisy at the grasp point | 3D error > 1.5 cm | Median depth over the box centre; fixed table plane as a prior | Use a known table plane from a one-off calibration |
| 4 | Detection misses the props | < 90% on the test set | Fine-tune on photos of the actual props, then convert for the BPU | Colour segmentation inside YOLO boxes |
| 5 | Grasps slip on round ornaments | < 80% success | Approach from straight above; slower close; soft gripper pads | Use only boxes and ornaments with hooks |

## Repository layout

```
elf101/
├── src/
│   ├── elf_camera/      # stereo capture + depth
│   ├── elf_detector/    # BPU YOLO node
│   ├── elf_locator/     # 2D + depth → 3D, calibration
│   ├── elf_task/        # task planner, web UI
│   └── elf_arm/         # SO-101 driver, IK, safety, e-stop
├── launch/              # ROS 2 launch files
├── models/              # model conversion scripts (no large binaries in git)
├── config/              # calibration, joint limits, workspace
├── docs/                # proposal, architecture, plan, results
└── tests/               # unit tests + trial logs
```
