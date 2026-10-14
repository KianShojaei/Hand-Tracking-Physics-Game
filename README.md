# Hand-Tracking Physics Game

I started this project as an experiment: could I use the tip of my index finger to draw lines on a live webcam feed and make a moving ball react to them? That simple idea grew into a small game built around hand tracking, collision detection, and real-time interaction.

The repository contains three stages of the project. The original prototype captures the initial idea; the structured revision reorganizes the code; and the creative revision explores additional ways to use the tracked hand during play. I have kept the versions side by side so the changes can be followed in both the source code and the gameplay recordings.

## Gameplay at a glance

The short recordings below show how the project behaves in practice. Screenshots for each version follow, with captions to identify the screen or mechanic being shown.

### 1. Original Prototype — 2024

**Source file:** `Source/original_prototype.py`

<table>
  <tr>
    <td align="center" width="33%">
      <img src="Image/original_prototype_start_page.png" alt="Original prototype start screen" width="100%" />
      <br /><sub><b>Start screen</b><br />The screen shown before a round begins.</sub>
    </td>
    <td align="center" width="33%">
      <img src="Image/original_prototype_game_page.png" alt="Original prototype gameplay screen" width="100%" />
      <br /><sub><b>Gameplay</b><br />The webcam feed and the original hand-controlled interaction.</sub>
    </td>
    <td align="center" width="33%">
      <img src="Image/original_prototype_end_page.png" alt="Original prototype end screen" width="100%" />
      <br /><sub><b>End screen</b><br />The result shown when the round ends.</sub>
    </td>
  </tr>
</table>

**Gameplay recording**

![Gameplay recording of the original prototype](GamePlay/original_prototype.gif)

### 2. Structured Revision — 2026

**Source file:** `Source/structured_revision_2026.py`

<table>
  <tr>
    <td align="center" width="50%">
      <img src="Image/structured_revision_start_page.png" alt="Structured revision start screen" width="100%" />
      <br /><sub><b>Start screen</b><br />The entry screen for the reorganized version.</sub>
    </td>
    <td align="center" width="50%">
      <img src="Image/structured_revision_game_page.png" alt="Structured revision gameplay screen" width="100%" />
      <br /><sub><b>Gameplay</b><br />The drawing-and-ball interaction in the structured version.</sub>
    </td>
  </tr>
</table>

**Gameplay recording**

![Gameplay recording of the structured revision](GamePlay/structured_revision.gif)

### 3. Creative Revision — 2026

**Source file:** `Source/creative_revision_2026.py`

<table>
  <tr>
    <td align="center" width="25%">
      <img src="Image/creative_revision_start_page.png" alt="Creative revision start screen" width="100%" />
      <br /><sub><b>Start screen</b><br />The starting point for the expanded version.</sub>
    </td>
    <td align="center" width="25%">
      <img src="Image/creative_revision_ball_hit1.png" alt="Creative revision ball interaction example one" width="100%" />
      <br /><sub><b>Ball interaction I</b><br />One example of the ball responding during play.</sub>
    </td>
    <td align="center" width="25%">
      <img src="Image/creative_revision_ball_hit2.png" alt="Creative revision ball interaction example two" width="100%" />
      <br /><sub><b>Ball interaction II</b><br />A second example of ball interaction.</sub>
    </td>
    <td align="center" width="25%">
      <img src="Image/creative_revision_shield_fist.png" alt="Creative revision fist-based shield" width="100%" />
      <br /><sub><b>Fist-based shield</b><br />A shield activated by a fist gesture.</sub>
    </td>
  </tr>
</table>

**Gameplay recording**

![Gameplay recording of the creative revision](GamePlay/creative_revision.gif)

## What changed between versions?

### Original prototype

The first version established the main loop: read frames from the webcam, locate the hand, use the index fingertip as an input point, and leave short-lived line segments on the screen. When the ball reaches a segment, its velocity is reflected so that the line acts as a temporary barrier. The game also includes collectible items, power-ups, lives, and a bomb hazard.

I have kept this version close to the original implementation. It is useful as a reference for understanding where the project began.

### Structured revision

The second version focuses on code organization and clearer responsibilities. It introduces a more explicit game-state flow and separates configuration, hand tracking, game objects, drawing, physics, and rendering into distinct components.

Other changes include clearer image-loading errors, more deliberate webcam and resource handling, restart and canvas-clear controls, difficulty progression, and collision checks against finite line segments. The aim here was to make the original idea easier to follow and maintain without changing its central interaction.

### Creative revision

The third version explores what changes when the tracked hand becomes more than a drawing input. The hand can act as a paddle, and a fist gesture can activate a temporary shield. This version also adds combo scoring, a second ball at higher difficulty, ball trails, particle effects, and additional visual feedback for collisions and item collection.

These additions make the interaction more varied, while keeping the same webcam-based foundation.

## How the interaction works

The main interaction is:

**Track the hand → draw a temporary barrier → let the ball react to the barrier → collect items and avoid hazards.**

In the creative revision, the hand can also interact with the ball directly as a paddle, and a fist gesture can trigger a shield.

At a high level, the processing loop follows this path:

```text
Webcam frame
    ↓
MediaPipe Hands
    ↓
Hand landmarks
    ↓
Screen coordinates and hand geometry
    ↓
Drawing / collision checks
    ↓
Ball response and game-state updates
    ↓
Rendering and on-screen feedback
```

The project brings together several practical topics: computer vision, hand tracking, 2D geometry, basic physics, collision detection, state management, and real-time rendering.

## Interaction comparison

| Interaction | Original Prototype | Structured Revision | Creative Revision |
|---|:---:|:---:|:---:|
| Index-finger drawing | ✓ | ✓ | ✓ |
| Temporary drawing barriers | ✓ | ✓ | ✓ |
| Ball reflection | ✓ | ✓ | ✓ |
| Bonus collection | ✓ | ✓ | ✓ |
| Power-ups and life power-up | ✓ | ✓ | ✓ |
| Bomb hazard | ✓ | ✓ | ✓ |
| Explicit game states | — | ✓ | ✓ |
| Hand used as a paddle | — | — | ✓ |
| Fist-based shield | — | — | ✓ |
| Combo scoring | — | — | ✓ |
| Multiple balls | — | — | ✓ |
| Particle effects and ball trail | — | — | ✓ |

## Physics and collision handling

The ball is represented by its position, velocity, and radius. When it hits a drawn barrier, the code uses the segment's normal to reflect the velocity vector. In the revised versions, collision checks consider the finite line segment rather than treating every stroke as an infinitely long line.

After a collision, the ball is moved slightly away from the barrier to help prevent repeated overlap with the same segment. The creative revision applies a related reflection approach to interaction with the hand-paddle.

This is a lightweight game-physics model, not a general-purpose physics engine; the focus is on making the interaction responsive and understandable.

## Hand tracking

The project uses MediaPipe Hands to estimate hand landmarks from webcam frames. In the original prototype, the index fingertip is the main input point. The structured revision places tracking in a dedicated `HandTracker` component, while the creative revision uses the landmark coordinates to build a convex hull for paddle interaction and applies a geometric heuristic to recognize a fist gesture.

Because the input comes from a camera, results can vary with lighting, hand occlusion, camera quality, and the position of the hand in the frame.

## Game states and controls

The structured and creative revisions separate the main states into a start screen, active gameplay, and a game-over screen. This makes the start and restart flow more explicit than in the original prototype.

The revised versions use these keyboard controls:

| Key | Action |
|---|---|
| `SPACE` | Start or restart the game |
| `D` | Start drawing |
| `S` | Stop drawing |
| `C` | Clear the drawing canvas |
| `R` | Switch drawing color to red |
| `P` | Switch drawing color to purple |
| `ESC` | Quit |

The original prototype has a smaller set of controls and remains in the repository as the earlier version.

## Run the project

### Requirements

- Python 3.10 or a version supported by the installed MediaPipe release
- A working webcam
- The packages listed in `requirements.txt`

### 1. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Or on macOS and Linux:

```bash
source .venv/bin/activate
```

### 2. Install the dependencies

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 3. Run a version

From the repository root, run the original prototype:

```bash
python Source/original_prototype.py
```

Or run one of the revised versions:

```bash
python Source/structured_revision_2026.py
python Source/creative_revision_2026.py
```

The scripts use the default webcam. If the camera cannot be opened, close other applications that may be using it. The five image assets used by the game are stored in `GameImages/`. Their paths are resolved relative to the repository, so the scripts do not depend on the current working directory.

## Repository layout

```text
Hand-Tracking-Physics-Game/
├── GameImages/
│   ├── ball.png
│   ├── bomb.png
│   ├── bonus.png
│   ├── life_powerup.png
│   └── powerup.png
├── GamePlay/
│   ├── original_prototype.gif
│   ├── structured_revision.gif
│   └── creative_revision.gif
├── Image/
│   ├── creative_revision_ball_hit1.png
│   ├── creative_revision_ball_hit2.png
│   ├── creative_revision_shield_fist.png
│   ├── creative_revision_start_page.png
│   ├── original_prototype_end_page.png
│   ├── original_prototype_game_page.png
│   ├── original_prototype_start_page.png
│   ├── structured_revision_game_page.png
│   └── structured_revision_start_page.png
├── Source/
│   ├── original_prototype.py
│   ├── structured_revision_2026.py
│   └── creative_revision_2026.py
├── .github/
│   └── workflows/
│       └── python-checks.yml
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

The three source files are kept separately rather than replacing one another. That makes it possible to compare the original implementation with the later changes directly.

## Tools and libraries

- **Python** — application logic
- **OpenCV** — webcam input and display
- **MediaPipe Hands** — hand-landmark tracking
- **NumPy** — numerical and geometric calculations
- **Pillow** — image loading and drawing

## Limitations and possible next steps

This project is an interactive prototype, and there are several areas that could be improved. Tracking quality depends on the camera and lighting, and a hand that is partly hidden can be harder to track. Fist recognition is based on a geometric heuristic rather than a separately trained gesture classifier. The physics model is intentionally simple.

Possible next steps include testing across a wider range of camera setups, profiling performance, improving gesture recognition, expanding collision handling, and separating rendering from game logic more fully.

## Project context

I use this project to explore how a computer-vision input can drive a real-time interaction. What began as drawing lines with an index fingertip became a way to experiment with geometry, physics, game-state design, and gesture-based mechanics in one small application.

The three versions are included because the progression is part of the project: the first demonstrates the idea, the second revisits its structure, and the third experiments with new interactions.

---

**Author:** Kian Shojaei  
**Repository:** [KianShojaei/Hand-Tracking-Physics-Game](https://github.com/KianShojaei/Hand-Tracking-Physics-Game)
