# Proposal: Elf-101

## The scenario

A table during the holidays: wrapped gifts in a few colours, some ornaments, a basket and a small tree. A person asks for something, and the arm finds it, picks it up and either hands it over or puts it where it belongs.

It's a Christmas demo, but the skill underneath is a general one: **find a named object with a camera and move it to a named place, on a low-cost arm, entirely on an edge board.** The same pipeline sorts parts on a workbench or tidies a classroom table.

## The user

- **At home or at an event:** someone who wants a fun, hands-on robot that does something useful when asked.
- **In a classroom:** students in Ghana learning robotics through Mikrobot Academy, where I teach. Elf-101 is meant to be something a school can rebuild: an open-source arm and a single board, with no cloud or GPU laptop needed.

## How you interact with it

1. You place objects anywhere on the marked table area.
2. You choose a task: by keyboard or a simple web page first, by voice as a stretch goal.
3. Elf-101 highlights what it sees on screen (live detection boxes and distances), then moves.
4. A physical stop button halts the arm at any time.

## What it can do

| Capability | How |
|---|---|
| Perceive | Stereo camera on the RDK X5; YOLO detection on the BPU for gifts (by colour) and ornaments |
| Locate | Stereo depth gives each detection a 3D position in the arm's frame |
| Decide | A small task planner picks the target and the drop location for the request |
| Act | Inverse kinematics plus smooth, speed-limited motion on the SO-101; open, grasp, lift, place |
| Stay safe | Speed and workspace limits in software, plus a GPIO stop button outside the software path |

## What's new compared with a stock demo

- **Detection drives a real arm.** Most RDK demos stop at boxes on a screen; here every detection ends in a physical grasp.
- **Depth instead of hand calibration.** The stereo camera replaces measuring the table height by hand, which was the weakest part of my earlier SO-101 projects.
- **Everything runs on the board.** No laptop and no cloud model in the loop.
- **It's measured.** Success targets are below, and the final README will report actual numbers from repeated trials.

## Measurable goals

These are targets set before building; the final results will be reported honestly against them.

| Metric | Target | How it's measured |
|---|---|---|
| Detection speed on the BPU | ≥ 15 FPS at 640×640 | Logged frame times over a 60 s run |
| Detection accuracy on the props | ≥ 90% of objects found in 50 test images | Hand-labelled test set |
| 3D position error | ≤ 1.5 cm at the grasp point | Compared against measured positions on a grid |
| Pick-and-place success | ≥ 16 of 20 trials (80%) | 20 trials with objects in random positions |
| Request to motion start | ≤ 1 s | Timestamps in the logs |
| Emergency stop | Arm stops within 200 ms of the button press | Video frame count plus logs |
