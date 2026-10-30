# Anteater Chess

A custom chess variant developed in **C**, featuring an interactive **GTK graphical user interface**, a computer opponent powered by minimax with alpha-beta pruning, and unique game mechanics built around the Anteater piece.

Developed by Team 21 for EECS 22L — Software Engineering Project in C at the University of California, Irvine.

## Overview

Anteater Chess is a variation of traditional chess played on an 8×10 board, introducing a new piece called the **Anteater** and replacing traditional pawns with ants.

The Anteater moves one square in any direction, similar to a king, but cannot threaten the opposing king. Instead, it specializes in capturing ants, with the ability to capture multiple adjacent ants in a single move.

The game supports both human-vs-computer and human-vs-human gameplay through a graphical interface. Players can select their preferred color, choose an AI difficulty level, and interact with the board using mouse controls.

The application integrates custom chess rules, AI decision-making, move validation, and graphical rendering into a modular C codebase.

## Features

### Gameplay
- **8×10 Chessboard:** Expanded board accommodating the additional Anteater pieces.
- **Custom Anteater Mechanics:** Unique movement and multi-ant capture behavior.
- **Human vs. Computer:** Play against an AI opponent.
- **Human vs. Human:** Two-player local gameplay.
- **Player Customization:** Select white or black pieces.
- **Move Validation:** Validates piece movement and captures according to Anteater Chess rules.
- **Special Moves:** Supports castling, en passant, and pawn promotion.
- **Check and Checkmate Detection:** Identifies threatened kings and evaluates game-ending conditions.
- **Stalemate Detection:** Recognizes positions where a player has no legal moves.

### Artificial Intelligence
- **Three Difficulty Levels:** Easy, Medium, and Hard.
- **Minimax Algorithm:** Evaluates possible moves by simulating future game states.
- **Alpha-Beta Pruning:** Reduces unnecessary search branches to improve decision-making efficiency.
- **Heuristic Board Evaluation:** Scores positions using piece values and tactical considerations.
- **Legal Move Generation:** Generates candidate moves while enforcing piece movement rules.

### Graphical User Interface
- **GTK-Based Interface:** Interactive chessboard with mouse-controlled piece movement.
- **Cairo Rendering:** Draws the chessboard, pieces, and move indicators.
- **Move Highlighting:** Highlights available moves for selected pieces.
- **Piece Promotion Interface:** Allows players to select a replacement piece.
- **Game Timers:** Displays countdown timers for each player.
- **Move History:** Displays recorded moves in official chess notation during gameplay.
- **Game Configuration:** Supports player color, game mode, and AI difficulty selection.

### Additional Functionality
- **Undo System:** Uses a stack-based move history to restore previous board states.
- **Move Logging:** Records gameplay actions in `chess_game.log`.


## Artificial Intelligence

The computer opponent uses different move-selection strategies depending on the chosen difficulty.

| Difficulty | Strategy |
|------------|----------|
| Easy | Random selection from available legal moves |
| Medium | Heuristic evaluation of immediate moves |
| Hard | Minimax search with alpha-beta pruning |

### Minimax Algorithm

The minimax algorithm searches possible future moves to determine which action produces the most favorable outcome.

The AI evaluates possible moves by:

1. Generating legal moves for the current player.
2. Simulating each candidate move on a copied board state.
3. Evaluating potential opponent responses.
4. Assigning scores to resulting positions.
5. Selecting the move with the strongest evaluated outcome.

### Alpha-Beta Pruning

Alpha-beta pruning improves minimax efficiency by eliminating branches that cannot influence the final decision.

- **Alpha:** Best score available to the maximizing player.
- **Beta:** Best score available to the minimizing player.
- **Pruning:** Stops exploring a branch when alpha is greater than or equal to beta.

This reduces the number of game states evaluated while preserving the minimax decision for the explored search depth.

### Board Evaluation

The AI uses weighted piece values to estimate the strength of a board position.

| Piece | Evaluation Value |
|-------|------------------|
| Pawn (Ant) | 100 |
| Knight | 320 |
| Bishop | 330 |
| Rook | 500 |
| Anteater | 525 |
| Queen | 900 |
| King | 20,000 |

These scores help the AI evaluate material advantages and determine favorable moves.

## Game Rules

### Board Layout

Anteater Chess uses an **8-row × 10-column** board.

The starting arrangement for each player's back rank is:

```text
R  N  B  A  Q  K  A  B  N  R
```

Where:

- R = Rook
- N = Knight
- B = Bishop
- A = Anteater
- Q = Queen
- K = King

Each player also begins with a row of ten ants (pawns).

### Anteater Movement

The Anteater introduces additional gameplay mechanics:

- Moves one square in any direction, similar to a king.
- Cannot threaten or capture the opposing king.
- Can capture ants using its specialized capture rules.
- Can capture multiple ants along a valid capture path.
- Supports additional capture opportunities when eligible enemy ants remain adjacent.

These mechanics introduce additional tactical considerations beyond traditional chess.

## Technologies Used

| Technology | Purpose |
|------------|---------|
| C | Core game logic and AI implementation |
| GTK | Graphical user interface and event handling |
| Cairo | 2D rendering of the board and pieces |
| GLib / GDK | GTK application support and graphics integration |
| GNU Make | Build automation |
| Git / GitHub | Version control and collaboration |

## Software Architecture

The application follows a modular architecture that separates responsibilities across several components.

### Core Game Logic

- `pieces.c / pieces.h` — Piece definitions, movement validation, capture rules, and special moves.
- `board.c / board.h` — Board initialization, piece placement, and board manipulation.
- `move.c / move.h` — Move execution, captures, castling, and en passant.
- `board_state.c / board_state.h` — Turn tracking, move generation, and interaction state.
- `board_bundle.c / board_bundle.h` — Shared game state and interface components.

### Artificial Intelligence

- `ai.c / ai.h` — AI move generation, heuristic evaluation, minimax, and alpha-beta pruning.

### Game Controller

- `game_controller.c / game_controller.h` — Processes player interactions and coordinates move execution.
- `input.c / input.h` — Parses text-based commands and validates input.

### Graphical Interface

- `gui.c / gui.h` — GTK application interface, event callbacks, dialogs, and gameplay controls.
- `render.c / render.h` — Board rendering, piece images, and move highlighting using Cairo.
- `color_gui.c / color_gui.h` — GUI styling and color definitions.

### Supporting Systems

- `undo.c / undo.h` — Stack-based move history and undo operations.
- `log.c / log.h` — Human-readable move logging.
- `save.c / save.h` — Game-state persistence routines.

## Build and Run

### Requirements

- C compiler (GCC or compatible)
- GNU Make
- GTK development libraries
- Cairo development libraries
- GLib / GDK dependencies

### Compilation

From the project root directory:

```bash
make
```

### Run

```bash
./bin/chess
```

For additional installation and configuration details, refer to the project's `INSTALL` file.

## How to Play

1. Launch the application.
2. Select a game mode.
3. Choose your preferred piece color.
4. If playing against the computer, select an AI difficulty.
5. Click one of your pieces to display its available moves.
6. Click a highlighted square to move the selected piece.
7. Continue playing until checkmate, stalemate, or another game-ending condition.

The interface also provides controls for game timers, promotion, move history, and undo functionality.

For detailed gameplay instructions, refer to `doc/Chess_UserManual.pdf`.

## Testing

The project includes tests for important game functionality.

| Test File | Purpose |
|-----------|---------|
| `test_rulecheck.c` | Validates piece movement rules |
| `test_boarddisplay.c` | Checks board initialization and ASCII display |
| `test_cpu_move.c` | Tests AI move selection and execution |

Testing covers scenarios such as:

- Valid and invalid piece movements.
- Initial board configuration.
- Board updates after moving pieces.
- AI selection of pieces belonging to the correct player.
- Prevention of illegal king captures.

## Project Documentation

Additional documentation is available in the `doc/` directory.

- `Chess_UserManual.pdf` — Gameplay instructions and user controls.
- `Chess_SoftwareSpec.pdf` — Software design and architecture documentation.
- `INSTALL` — Installation and build instructions.

## Development Team

Developed by **Team 21** at the University of California, Irvine.

- Naveen Khan
- Mia Ness
- Austin Nguyen
- Queency Tan
- Aman Macwan
- Gurvin Dhanjal

**Course:** EECS 22L — Software Engineering Project in C  
**Instructor:** Prof. Rainer Doemer  
**Term:** Spring 2026

## Project Information

- **Language:** C
- **GUI Framework:** GTK
- **Graphics:** Cairo
- **AI Algorithms:** Minimax, Alpha-Beta Pruning, Heuristic Evaluation
- **Version:** 1.1
- **Original Release:** April 26, 2026
