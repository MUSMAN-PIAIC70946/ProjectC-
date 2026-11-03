# THE LAST TRAIN 🚂

**THE LAST TRAIN** is a deterministic 2D grid and text-based rescue simulation game developed in C++17. The game models complex player navigation across a 5-floor facility containing 20 rooms to rescue an injured passenger and a stranded animal. Along the journey, players must discover hidden clue digits, decipher a security cabinet to obtain the Train Key, restore vertical transit systems, and prepare the escape train before depleting Energy or Time.

This project is built deterministically without using OOP classes, relying instead on clean, modular functions, multi-dimensional structures, custom structs, and memory-safe pointer operations.

---

## 🎮 Game Architecture & Objective Mechanics

### 🏢 Building Layout & Designations
The facility contains 5 floors with 4 rooms per floor (20 rooms total). Each room is uniquely tracked as `F<floor>-R<room>`:
*   **Floor 1:** F1-R1 (Entrance Lobby) | F1-R2 (Reception) | F1-R3 (Waiting Area) | F1-R4 (Utility Room)
*   **Floor 2:** F2-R1 (Lounge) | F2-R2 (Archive) | F2-R3 (Lab) | F2-R4 (Meeting Room)
*   **Floor 3:** F3-R1 (Stairwell Annex) | F3-R2 (Security Vestibule) | F3-R3 (Animal Room) | F3-R4 (Control Room)
*   **Floor 4:** F4-R1 (First Aid Station) | F4-R2 (Medical Room) | F4-R3 (Maintenance Corridor) | F4-R4 (Office)
*   **Floor 5:** F5-R1 (Security Room) | F5-R2 (Train Boarding Platform) | F5-R3 (Storage Room) | F5-R4 (Observation Deck)

### 🚪 Connectivity & Navigation Rules
*   **Ordinary Doors (D):** Standard bidirectional travel between adjacent rooms.
*   **Windows (W):** Horizontal vaulting paths connecting F1-R1↔F1-R2, F3-R2↔F3-R3, F4-R1↔F4-R2, and F5-R3↔F5-R4.
*   **Staircases (S1, S2, S3):** Vertically connects room segments (F1↔F2, F2↔F3, and F4↔F5).
*   **Solid Wall (#):** An impassable barrier that completely blocks passage between F4-R2 and F4-R3.
*   **Lift 1 (L1):** Operates unconditionally to transition between Floor 1 and Floor 2.
*   **Lift 2 (L2):** High-speed lift connecting F2-R4, F4-R4, and F5-R4. Normal operation requires inserting a **Fuse** at the Control Panel (F3-R4) and holding a **Lift Pass**. Prior to repair, an **Emergency Access Card** grants a maximum of 3 emergency trips.

### ⚖️ Resource Economics
Energy and Time are strictly clamped between 0 and 100. If either resource drops to 0 before the train is boarded, the mission immediately ends in failure.
*   **Ordinary Movement (Door/Stair):** Cost -2 Energy, -1 Time
*   **Window Traversal:** Cost -3 Energy, -2 Time
*   **Lift Journey (L1/L2):** Cost -4 Energy, -3 Time
*   **Room Inspection / Item Pickup / Puzzle Interaction:** Cost -1 Energy, -1 Time
*   **Energy Packs / Emergency Toolkit / Observation Token:** Restore +25 Energy, +15 Energy, and +15 Time respectively upon pickup.

### 🏆 Scoring Breakdown (520 Points Maximum)
*   **Master Key Collected:** +10 points
*   **Clue Digits Found (4, 2, 7):** +10 points each (+30 total)
*   **Cabinet Unlocked (Code 427):** +30 points
*   **Train Key Acquired:** +10 points
*   **Fuse Installed at Panel (F3-R4):** +20 points
*   **Lift Pass Collected (F3-R4):** +20 points
*   **Injured Passenger Rescued (F4-R1):** +100 points
*   **Cat Secured (F3-R3):** +100 points
*   **Train Prepared & Boarded (F5-R2):** +100 points
*   **Full Mission Victory Bonus:** +100 points

---

## 📁 Repository Directory Structure

The project codebase strictly follows the layout specified below:

```text
├── data/
│   ├── building.txt               # Static ASCII building map topology and configuration
│   └── initial-state.txt          # Starting configurations and game metrics
├── docs/
│   ├── Project_Diagram.jpg        # Architectural system overview diagram
│   ├── Flowchart.png              # Logic flow and state transition flowchart
│   └── ConsoleUI.png              # Console user interface design mockup
├── include/
│   ├── config.h                   # Global constants and simulation parameters
│   ├── state.h                    # Global game state struct definitions
│   ├── level.h                    # Map loading and grid structures
│   ├── simulator.h                # Core input loop and game cycle handlers
│   ├── physics.h                  # Navigation rules, costs, and boundary constraints
│   ├── switches.h                 # Puzzles, keys, and puzzle mechanic definitions
│   ├── logger.h                   # Game event logging utilities
│   ├── util.h                     # String processing and helper functions
│   └── viewer_iface.h             # Console interface layout contracts
├── out/
│   ├── savegame.txt               # Serialized snapshot of a saved session
│   └── game_summary.txt           # Post-game session metrics and audit report
├── src/
│   └── core/
│       ├── main_core.cpp          # Engine entry point and primary game loop
│       ├── level.cpp              # Map parsing and file loading implementation
│       ├── simulator.cpp          # Player commands processing controller
│       ├── physics.cpp            # Move tracking and resource deduction rules
│       ├── switches.cpp           # Key validation and lock puzzle logic
│       ├── logger.cpp             # Session summary and state saving writers
│       ├── util.cpp               # Shared string and data formatting helpers
│       └── viewer_iface_core.cpp  # Dynamic dashboard console printer
└── README.md                      # Documentation manual
```

---

## 🛠️ Build & Run Instructions

### Prerequisites
*   **Compiler:** `g++` supporting the **C++17 standard** or newer.

### Compilation Command
Compile the modular subsystems cleanly from the repository root folder using the required optimization and warning flags:

```bash
g++ -std=c++17 -Wall -Wextra -Iinclude src/core/*.cpp -o last_train
```

### Execution
Run the compiled binary directly from the root workspace directory to guarantee relative path lookups track correctly into localized data assets:

```bash
./last_train
```

---

## 🧪 Proof of Determinism & Verification

Because this framework relies completely on clean functional state design, discrete multi-dimensional arrays, and struct blocks without randomized runtime values, any identical sequence of command inputs yields an identical result down to the single resource unit:
1.  Launch the simulation using standard starting parameters.
2.  Input a matching trajectory of movement patterns and puzzle combinations.
3.  Check the resulting post-session balances recorded in `./out/game_summary.txt`.
4.  Re-run the exact sequence across multiple trials; remaining Energy, remaining Time, and final score metrics will match identically.

