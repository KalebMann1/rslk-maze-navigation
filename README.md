# RSLK Maze Navigation (TI SimpleRSLK, Bump Switch & Encoders)

This repository contains a set of Arduino-style sketches for solving mazes
using the TI SimpleRSLK robot platform. Two main strategies are implemented:

1. **Reactive navigation** using bump switches
2. **Pre-planned navigation** using wheel encoders and distance/angle control

The code was originally developed for a robotics / embedded systems lab.

---

## Hardware Platform

- **Robot:** TI SimpleRSLK / Romi-based platform
- **MCU:** TI LaunchPad (MSP432) using the SimpleRSLK library
- **Sensors & Actuators:**
  - Bump switches on the front bumper
  - DC motors with quadrature encoders
  - On-board push button and RGB LED for user interface

---

## Code Structure

- `Maze1_BumpSwitch.ino`  
  Reactive solution for Maze 1. The robot:
  - Drives forward until a bump switch is triggered
  - Uses which switch was hit (0–5) to decide how far and in which
    direction to turn
  - Restarts forward motion after each avoidance maneuver

- `Maze1_Distance.ino`  
  Encoder-based solution for Maze 1. The robot:
  - Uses helper functions `driveDistance()` and `turnAngle()`
  - Executes a fixed sequence of moves (drive/turn) to follow a planned path
    and complete the maze.

- `Maze2_BumpSwitch.ino`  
  Reactive solution for a different maze layout, with tuned
  turn angles and behaviors for each bump switch.

- `Maze2_Distance.ino`  
  Pre-planned encoder solution for Maze 2, using a different sequence of
  distances and angles to navigate the new maze geometry.

---

## Control Approach

### Bump-Switch Navigation

- Motors are set to a default forward speed.
- Inside the main loop, the code continuously checks all bump switches:
  - Side hits → small corrective turns
  - Center hits → larger turns or backing up first, then turning
- After a collision response, the robot resumes forward motion.

This approach demonstrates **reactive obstacle avoidance** based purely
on contact sensors.

### Distance / Angle Navigation

- `driveDistance(inches)` converts inches → encoder ticks and drives until
  the target tick count is reached.
- `turnAngle(degrees)` converts desired angle → encoder ticks and spins
  the robot in place.
- The maze solution is encoded as a sequence of `driveDistance()` and
  `turnAngle()` calls in `loop()`.

This demonstrates **open-loop path planning** using wheel encoders.

---

## Skills Demonstrated

- Embedded C/C++ with the TI SimpleRSLK library
- Working with:
  - DC motor control (speed + direction)
  - Wheel encoders for distance/angle estimation
  - Bump switches for collision detection
- Designing and tuning motion primitives (`driveDistance`, `turnAngle`)
- Comparing reactive vs pre-planned navigation strategies on a real robot
