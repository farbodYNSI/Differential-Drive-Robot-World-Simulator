# Differential Drive Robot World Simulator

This repository contains a small Python module that simulates a
**differential drive robot** in a 2D world and visualizes its motion
using OpenCV.

The main class is `world`, which keeps track of the robot's pose
`(x, y, θ)`, updates it based on wheel speeds, and renders:

-   The robot (from an image, e.g. `nav.png`)
-   The global coordinate axes
-   The robot's path (trajectory)
-   Optional target points

## Features

-   Differential-drive kinematic model
-   Real-time simulation step (`tick()`)
-   Angle normalization to `[-π, π]`
-   OpenCV-based visualization of:
    -   Robot orientation
    -   Path history
    -   Target points
    -   Global coordinate axes
-   Helper methods:
    -   `pos()`
    -   `visualize()`
    -   `draw_target(...)`
    -   `draw_path(...)`

## Files

-   `world.py` --- main module implementing the simulator
-   `nav.png` --- robot sprite (required for visualization)

## Installation

``` bash
pip install numpy opencv-python
```

## Usage

### Create a world

``` python
from world import world

w = world(wheel_length=0.5)
```

### Simulate motion

``` python
for _ in range(100):
    w.tick(left_speed=1.0, right_speed=1.0, time=0.2)

print(w.pos())  # (x, y, theta)
```

### Visualize

``` python
import cv2
from world import world

w = world(wheel_length=0.5)

for _ in range(200):
    w.tick(left_speed=0.5, right_speed=1.0, time=0.2)

    frame = w.visualize()
    frame = w.draw_path(frame)

    targets = [(20, 20), (-30, 10)]
    frame = w.draw_target(targets, frame)

    cv2.imshow("Robot World", frame)
    if cv2.waitKey(10) & 0xFF == 27:
        break

cv2.destroyAllWindows()
```

## Coordinate System

-   World is visualized on an `800×800` canvas
-   Origin `(0, 0)` is the center of the map
-   `(x, y)` are in world units
-   `θ` is in radians, normalized to `[-π, π]`

