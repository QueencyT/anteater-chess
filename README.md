
# Anteater Chess
## Overview

Anteater Chess is a custom chess variant developed in C, featuring an expanded 8×10 chessboard, unique gameplay mechanics, an interactive graphical interface built with GTK4 , and an AI-powered opponent utilizing minimax with alpha-beta pruning.

The game introduces a new piece called the Anteater, which moves one square in any direction, similar to a king, but cannot threaten the opposing king. Instead, it specializes in capturing ants (pawns), introducing unique capture mechanics that add a new layer of strategy to traditional chess.

Players can compete against AI opponents with three difficulty levels (Easy, Medium, and Hard) or challenge another player locally. The graphical interface provides interactive piece selection, legal move highlighting, player timers, move history log, and support for special chess moves.

**Developed collaboratively by a six-member team at the University of California, Irvine**

**Team-Members:**
- **Queency Tan** 
- **Austin Nguyen**
- **Mia Ness**
- **Aman**
- **Gurvin Singh**
- **Naveen Islame**
  
---

## My Contributions

As a member of the six-person development team, I was primarily responsible for graphical user interface development, undo functionality, and integration with the core game logic.

### Graphical User Interface Development

- Led the development of the graphical chess interface using GTK4 and Cairo, including the interactive 8×10 chessboard, piece rendering, and graphical move indicators.
- Implemented mouse-based piece selection and move highlighting to display available moves and provide visual feedback during gameplay.
- Developed GUI components for player timers, move history log, pawn promotion, game configuration, and gameplay dialogs.
- Integrated the graphical interface with the underlying chess engine to synchronize player interactions, board updates, and game-state changes.

### Undo Functionality

- Implemented a stack-based undo system to record executed moves and restore previous board positions.
- Designed move-history records to preserve information about moved pieces, captured pieces, and their original positions.
- Extended undo handling to support special moves such as castling by restoring rook positions and previous movement states.

### Code Integration

- Contributed to the integration of independently developed game components into the main application.
- Worked with shared data structures and interfaces used across the board, game logic, and graphical modules.
- Assisted with debugging and resolving issues involving interconnected gameplay components.

---
## Gameplay Showcase
https://github.com/user-attachments/assets/7030dde6-c27d-4ebb-a2dc-21d6c28f6ec6

<sup>Anteater Chess features an interactive graphical interface built with GTK4 and Cairo, supporting mouse-controlled piece movement and visual highlighting of legal moves. The interface also includes player countdown timers, a scrollable move history, pawn promotion dialogs, undo functionality, and game-over notifications, providing an intuitive and engaging gameplay experience.</sup>

## Undo Showcase
https://github.com/user-attachments/assets/47d2cd57-016b-49fc-926f-fc434b547292

<sup>Undo functionality allowing players to reverse previous moves and restore the board to its earlier state. Implemented using a stack-based move history, the system tracks piece movements, captured pieces, and special moves such as castling, ensuring that changes are accurately reversed. The undo feature is integrated with the GTK4 interface, allowing players to revert moves through a simple button click.</sup>


---
## Key Features

- **Custom Chess Variant:** Expands traditional chess to an 8×10 board with two additional Anteater pieces per player.
- **Unique Anteater Mechanics:** Introduces specialized ant-capture behavior, including chain-capture opportunities.
- **GTK4 Graphical Interface:** Provides interactive chess gameplay through graphical controls and mouse input.
- **Cairo Rendering:** Displays the chessboard, custom piece artwork, and highlighted move destinations.
- **AI-Controlled Opponent:** Supports computer gameplay with Easy, Medium, and Hard difficulty settings.
- **Minimax with Alpha-Beta Pruning:** Evaluates future board positions while reducing unnecessary search branches.
- **Legal Move Validation:** Implements movement and capture rules for traditional pieces and the Anteater.
- **Special Chess Moves:** Includes castling, en passant, and pawn promotion.
- **Game-State Management:** Tracks turns, move history, piece selection, and game-ending conditions.
- **Undo Functionality:** Uses a stack-based system to restore previous board positions.
- **Player Timers:** Tracks remaining time for each player.
- **Move Logging:** Records gameplay actions in a human-readable log file.

---

## System Architecture

The project follows a modular architecture that separates chess rules, board management, AI decision-making, graphical rendering, and supporting gameplay systems.

### 1. <ins>Board and Game-State Management</ins>

The board-management system maintains the current chess position and coordinates changes to the game's state.

The chessboard is represented as an 8×10 two-dimensional array of pointers to piece objects.

Its responsibilities include:

- Initializing the board and placing pieces in their starting positions.
- Retrieving, adding, removing, and relocating pieces.
- Tracking selected pieces and possible move destinations.
- Maintaining turn progression and previous-move information.
- Coordinating state updates with the graphical interface.

The `Board`, `Board_State`, and `Board_Bundle` structures separate piece placement, gameplay state, and shared application context.

This organization allows the game logic and graphical interface to access consistent board information.

### 2. <ins>Chess Rules and Move Validation</ins>

The rules engine determines whether a piece can move or capture according to the game's rules.

Each piece uses a structure containing function pointers for movement and capture validation.

The system supports:

- Traditional chess piece movement.
- Detection of blocked movement paths.
- Validation of captures against opposing pieces.
- Castling and en passant.
- Pawn promotion.
- Check and game-ending condition detection.
- Custom Anteater movement and capture rules.

The use of function pointers enables piece-specific movement behavior while maintaining a shared interface for rule validation.

### 3. <ins>Artificial Intelligence</ins>

The computer opponent evaluates possible chess moves and selects an action based on the configured difficulty.

The AI supports three decision-making strategies:

| Difficulty | Strategy |
|------------|----------|
| Easy | Random legal move selection |
| Medium | Immediate heuristic move evaluation |
| Hard | Minimax search with alpha-beta pruning |

#### Minimax Algorithm

The minimax algorithm explores possible future board states by alternating between maximizing and minimizing players.

The AI:

1. Generates candidate legal moves.
2. Simulates moves using copied board states.
3. Evaluates resulting positions.
4. Recursively considers opponent responses.
5. Selects the move with the strongest evaluated outcome.

#### Alpha-Beta Pruning

Alpha-beta pruning improves minimax efficiency by eliminating branches that cannot affect the final decision.

The algorithm maintains two bounds:

- **Alpha:** The best score available to the maximizing player.
- **Beta:** The best score available to the minimizing player.

When alpha is greater than or equal to beta, the remaining moves in that branch are pruned.

This reduces unnecessary board evaluations without changing the minimax result at the selected search depth.

#### Heuristic Evaluation

The AI assigns values to different pieces to evaluate material advantages.

| Piece | Value |
|-------|-------|
| Ant (Pawn) | 100 |
| Knight | 320 |
| Bishop | 330 |
| Rook | 500 |
| Anteater | 525 |
| Queen | 900 |
| King | 20,000 |

The evaluation system uses these values and additional tactical considerations to estimate the strength of a position.

### 4. <ins>GTK Graphical Interface</ins>

The graphical interface is implemented using GTK4 with Cairo-based rendering.

The interface handles:

- Chessboard and piece rendering.
- Mouse-based piece selection and movement.
- Legal move highlighting.
- Player color and difficulty selection.
- Game-mode configuration.
- Promotion dialogs.
- Player timers and move history.
- Checkmate and stalemate notifications.

GTK event callbacks translate user interactions into board coordinates and forward them to the game controller.

The rendering module uses Cairo to draw the board, display piece images, and update highlighted squares.

This design separates visual presentation from the underlying chess rules and state-management logic.

### 5. <ins>Move Execution and Game Controller</ins>

The game controller processes player actions and coordinates updates across the application.

Its responsibilities include:

- Receiving board interactions.
- Selecting pieces and generating available moves.
- Validating requested destinations.
- Executing regular and special moves.
- Updating turn and move history.
- Triggering AI-controlled turns.
- Coordinating board redraws and game-state updates.

The move execution module handles normal moves, captures, castling, en passant, and specialized Anteater captures.

Together, these components connect the rules engine, board state, AI, and user interface.

### 6. <ins>Undo and Move Logging</ins>

The project includes supporting systems for recording gameplay and reversing moves.

#### Undo System

The undo system maintains a stack of move records.

Each record stores relevant information about the move, including the moving piece, captured piece, and previous movement state.

When undo is requested, the system restores the recorded pieces to their previous positions.

Additional information is retained for special moves such as castling.

#### Move Logging

The logging module records gameplay actions in `chess_game.log`.

Recorded events include:

- Regular moves.
- Piece captures.
- Castling.
- En passant.
- Pawn promotion.
- Check and checkmate.
- Game-ending events.

The resulting log provides a human-readable record of gameplay.

---

## Core Components

The application is organized into reusable C modules.

| Component | Responsibility |
|-----------|----------------|
| `board` | Manages the chessboard, piece placement, and board operations |
| `board_state` | Tracks selected pieces, valid destinations, and gameplay state |
| `board_bundle` | Connects board data, game state, and graphical interface components |
| `pieces` | Defines chess pieces, movement rules, and capture validation |
| `move` | Executes moves, captures, and special chess actions |
| `ai` | Implements computer move selection, minimax, and alpha-beta pruning |
| `game_controller` | Processes user interactions and coordinates gameplay |
| `gui` | Builds the graphical interface and handles GTK events |
| `render` | Draws the board, pieces, and highlights using Cairo |
| `color_gui` | Defines interface colors and visual styling |
| `undo` | Maintains move history and restores previous positions |
| `log` | Records gameplay actions in a human-readable file |
| `input` | Parses and validates text-based gameplay commands |
| `save` | Contains routines for saving and loading game-state data |

---

## Build and Run

### Requirements

The application uses C and GTK4 and is intended for an environment with the required GTK and Cairo development libraries.

- GCC or compatible C compiler
- GNU Make
- GTK4 development libraries
- Cairo
- GLib and GDK dependencies

### 1. Clone the Repository

```bash
git clone https://github.com/QueencyT/anteater-chess.git
cd anteater-chess
```

### 2. Compile the Project

```bash
make
```

### 3. Run the Application

```bash
./bin/chess
```

Additional build instructions and configuration details are available in the project's `INSTALL` file.

---

## Testing

The project includes test programs covering board representation, piece movement validation, and AI behavior.

| Test File | Purpose |
|-----------|---------|
| `test_boarddisplay.c` | Verifies board initialization and ASCII board display |
| `test_rulecheck.c` | Tests legal and illegal piece movements |
| `test_cpu_move.c` | Tests computer move selection and execution |

Testing scenarios include:

- Checking valid and invalid rook movements.
- Verifying the initial 8×10 board configuration.
- Confirming that pieces are moved to the expected board positions.
- Checking that AI moves use pieces belonging to the computer player.
- Preventing illegal king captures during AI gameplay.

These tests provide targeted validation of important gameplay components.

---

## Project Structure

```text
anteater-chess/
├── src/
│   ├── main.c
│   ├── enum.h
│   ├── ai.c / ai.h
│   ├── board.c / board.h
│   ├── board_state.c / board_state.h
│   ├── board_bundle.c / board_bundle.h
│   ├── pieces.c / pieces.h
│   ├── move.c / move.h
│   ├── game_controller.c / game_controller.h
│   ├── input.c / input.h
│   ├── undo.c / undo.h
│   ├── log.c / log.h
│   ├── save.c / save.h
│   ├── test_boarddisplay.c
│   ├── test_rulecheck.c
│   ├── test_cpu_move.c
│   └── gui/
│       ├── gui.c / gui.h
│       ├── render.c / render.h
│       ├── color_gui.c / color_gui.h
│       └── resources/
├── doc/
│   ├── Chess_UserManual.pdf
│   └── Chess_SoftwareSpec.pdf
├── Makefile
├── INSTALL
└── README.md
```

---

## Project Information

**Project:** Anteater Chess  
**Language:** C  
**GUI Framework:** GTK4  
**Rendering:** Cairo  
**AI Algorithms:** Minimax, Alpha-Beta Pruning  
**Course:** EECS 22L, University of California, Irvine  
**Team:** Team 21  
**Original Release:** April 2026

### Contributors

Anteater Chess was developed collaboratively by Team 21.

**Portfolio maintained by:** Queency Tan (`QueencyT`, formerly `qatan-glitch`)

