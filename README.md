# 🎮 Game Arcade — Minimax AI

A collection of nine classic strategy games playable in the browser, each powered by a different AI algorithm. No server, no dependencies, no installation — just open an HTML file and play.

**Live:** [gunjanak.github.io/Chess-Min-Max](https://gunjanak.github.io/Chess-Min-Max/)

---

## Games

### ♟️ Chess
**File:** `chess_minimax.html`

The full game of chess with all rules implemented.

**How to play:** Click a piece to select it, then click a highlighted square to move. You play as White (or Black — configurable). The game ends on checkmate, stalemate, or draw.

**Rules included:** Castling (kingside & queenside), en passant, pawn promotion (choose piece via modal), check/checkmate/stalemate detection.

**Algorithm:** Minimax with Alpha-Beta Pruning
- Searches the game tree to a configurable depth (1–5)
- **Piece-Square Tables (PST)** — each piece has a positional bonus matrix encouraging good squares (knights prefer the center, kings hide in corners, pawns advance)
- **Move ordering** — captures searched first to improve pruning efficiency
- Evaluation = material value + positional score

---

### 🔴 Connect Four
**File:** `connect_four.html`

Drop coloured discs into a 7×6 grid and connect four in a row.

**How to play:** Click any column (or the arrow above it) to drop your disc. Connect four discs horizontally, vertically, or diagonally to win.

**Algorithm:** Minimax with Alpha-Beta Pruning
- Configurable depth (1–8)
- **Center-column ordering** — columns searched from center outward (strongest Connect Four heuristic)
- **Window scoring** — evaluates every group of four cells, scoring 2-in-a-row, 3-in-a-row, open-fours
- Disc-drop animation on each move

---

### ⬛ Othello (Reversi)
**File:** `othello.html`

Flip your opponent's discs to control the board.

**How to play:** Click any highlighted cell to place your disc. All opponent discs in a straight line between your new disc and an existing one are flipped to your colour. Most discs at the end wins. If you have no valid move your turn is skipped automatically.

**Algorithm:** Minimax with Alpha-Beta Pruning
- Configurable depth (1–8)
- **Positional weight table** — corners worth 120 points, cells next to corners heavily penalised (−40), edges positive
- **Mobility scoring** — rewards having more legal moves than the opponent
- **Corner bonus** — extra weight on securing all four corners
- **Move ordering** — high-weight squares searched first
- 3D coin-flip animation when discs are flipped

---

### 🔴 Checkers (Draughts)
**File:** `checkers.html`

Classic diagonal capture game on an 8×8 board.

**How to play:** Click a red piece to select it, then click a highlighted square to move. Diagonal moves only. Captures are mandatory — if a jump is available you must take it. Chain multiple jumps in one turn. Reach the opponent's back rank to promote to King (moves in all 4 diagonal directions).

**Algorithm:** Minimax with Alpha-Beta Pruning
- Configurable depth (1–10)
- **Piece values** — Man = 100, King = 300
- **Advancement bonus** — men rewarded for pushing toward promotion
- **Back-row defense** — bonus for keeping the back rank intact
- **Center bonus for Kings** — active kings score higher near the center
- **Move ordering** — captures searched before simple moves; kings before men
- Multi-jump chains handled correctly including mid-jump promotion

---

### 🔷 Hex
**File:** `hex.html`

Connect your two sides of the board before your opponent does.

**How to play:** Blue connects Top to Bottom; Red connects Left to Right. On each turn place one stone anywhere on the board. Hex has no draws — one player always wins. Blocking your opponent is done simply by filling cells they need.

**Board sizes:** 7×7 (easy), 9×9 (standard), 11×11 (hard)

**Algorithm:** Negamax + Iterative Deepening
- **Negamax** — cleaner formulation of Minimax; since Hex is zero-sum, negating the opponent's score gives your score
- **Iterative Deepening** — searches depth 1, 2, 3… until the time budget runs out. Always has a best move ready; uses time intelligently
- **Dijkstra evaluation** — measures the shortest virtual connection path for each player. Smaller distance = better position. Score = `oppDist − myDist`
- **Move ordering** — center cells and cells adjacent to own stones searched first to maximise alpha-beta cutoffs
- **Opening** — plays center on move 1 (provably strong in Hex)
- Configurable time budget (1–5 seconds)

---

### 🪨 Mancala (Kalah)
**File:** `mancala.html`

Ancient seed-sowing strategy game.

**How to play:** On your turn click one of your pits (south row) with seeds. Seeds are distributed counter-clockwise one per pit, skipping the opponent's store. **Extra turn:** if your last seed lands in your store, go again. **Capture:** if your last seed lands in your own empty pit, you steal all seeds from the opposite pit. When one side is emptied the remaining seeds go to that player's store. Most seeds wins.

**Seed options:** 3 (fast), 4 (standard), 6 (deep)

**Algorithm:** Negamax + Alpha-Beta + Iterative Deepening + Transposition Tables
- **Negamax** — handles extra-turn recursion cleanly (same player moves again, no sign flip needed)
- **Iterative Deepening** — depth 1, 2, 3… stops at time budget; always has the best move found so far
- **Transposition Tables + Zobrist Hashing** — Mancala positions repeat frequently (same seed counts via different move orders); caching gives a large speed boost especially during extra-turn chains
- **Extra-turn move ordering** — extra-turn moves searched first, then captures, then seed count. Dramatically improves alpha-beta cutoff rate
- Configurable time budget (1–6 seconds)

---

### 🪵 Nim
**File:** `nim.html`

Remove sticks from heaps — the player who takes the last stick wins (or loses in Misère mode).

**How to play:** Click sticks in any one heap to select how many to remove (you can take any number from one heap per turn). Click "Take Selected" to confirm. In **Normal** mode the last player to take wins. In **Misère** mode the last player to take loses.

**Presets:** Classic (1,3,5,7), Standard (3,5,7), Small (1,2,3), Large (5,7,9,11), Random

**Algorithm:** Negamax + Perfect Mathematical Strategy (XOR / Nim-sum)
- **Nim-sum theorem** — a position is a guaranteed loss for the player to move if and only if the XOR of all heap sizes equals 0 (called a P-position)
- The AI always moves to leave nim-sum = 0, playing perfectly
- **Negamax with Alpha-Beta** runs alongside to verify
- **Misère adjustment** — when all heaps are size ≤ 1, strategy flips: leave an odd number of single sticks for the opponent
- The **live XOR display** shows you the nim-value and whether you or the AI has the advantage after every move — great for learning the math

---

### ⚫ Gomoku (Five in a Row)
**File:** `gomoku.html`

Place stones on a 15×15 board and connect five in a row.

**How to play:** Click any intersection on the board to place your stone. Black always moves first. Connect five or more stones horizontally, vertically, or diagonally to win. Hover to preview stone placement.

**Board sizes:** 13×13, 15×15 (standard)

**Algorithm:** Negamax + Alpha-Beta + Iterative Deepening + Threat Detection
- **Candidate generation** — only cells within radius 2 of existing stones are considered, reducing 200+ candidates to ~20–30 per move
- **Immediate win/block** — checks for 1-move wins and forced blocks before entering the search tree
- **Pattern evaluation** — scores every window of 5: five-in-a-row (1,000,000), open-four (50,000), four (10,000), open-three (5,000), open-two (100). Defense weighted 1.1× to prevent pure attacking play
- **Threat-based move ordering** — candidates pre-scored and sorted so the strongest moves are searched first, maximising alpha-beta pruning
- **Iterative Deepening** — depth 1–8 within time budget (Easy 1.5s, Medium 2.5s, Hard 4s)
- Ghost stone preview on hover; golden win line drawn on game over

---

### 🚢 Battleship
**File:** `battleship.html`

Place your fleet and sink the enemy's before they sink yours.

**How to play:**
1. **Placement phase** — click your grid to place each ship. Press **R** (or the Rotate button) to toggle horizontal/vertical orientation. Click "Random Place" to fill remaining ships automatically. Click "Start Battle" when ready.
2. **Battle phase** — click the enemy grid to fire. A hit earns another shot; a miss hands the turn to the AI. Sink all 5 enemy ships to win.

**Fleet:** Carrier (5), Battleship (4), Cruiser (3), Submarine (3), Destroyer (2)

**Algorithm:** Probability Density + Hunt/Target with Parity Optimisation
- **Easy** — fires randomly at any untouched cell
- **Medium (Hunt/Target)** — in *hunt* mode uses checkerboard parity (skips half the board since no ship is smaller than 2). On a hit switches to *target* mode, queuing adjacent cells to systematically sink the ship
- **Hard (Probability Density)** — for every remaining ship size and every valid board position, counts how many ship placements cover each cell. Fires at the highest-probability cell. After a hit, overlapping placements receive a bonus multiplier so the AI rapidly converges. Parity applied during hunt mode. The **live heatmap** (toggleable) shows this probability cloud in real time

---

## Algorithms Used

| Game | Algorithm |
|---|---|
| Chess | Minimax + Alpha-Beta + PST |
| Connect Four | Minimax + Alpha-Beta |
| Othello | Minimax + Alpha-Beta |
| Checkers | Minimax + Alpha-Beta |
| Hex | Negamax + Iterative Deepening |
| Mancala | Negamax + Alpha-Beta + ID + Transposition Tables |
| Nim | Negamax + XOR Perfect Strategy |
| Gomoku | Negamax + Alpha-Beta + ID + Threat Detection |
| Battleship | Probability Density + Hunt/Target |

---

## Running Locally

No build step needed. Just open any file directly in a browser:

```bash
git clone https://github.com/gunjanak/Chess-Min-Max.git
cd Chess-Min-Max
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or serve with Python for a cleaner experience:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

---

## File Structure

```
index.html            ← Game arcade landing page
chess_minimax.html    ← Chess
connect_four.html     ← Connect Four
othello.html          ← Othello
checkers.html         ← Checkers
hex.html              ← Hex
mancala.html          ← Mancala
nim.html              ← Nim
gomoku.html           ← Gomoku
battleship.html       ← Battleship
README.md             ← This file
```

Each game is a single self-contained HTML file with no external dependencies.

---

*Built with vanilla HTML, CSS and JavaScript. All AI runs entirely in your browser.*
