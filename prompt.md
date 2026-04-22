# MISSION: DEVELOP "CỜ GÁNH" AI AGENT
**Role:** Expert AI/Game-Playing Software Engineer.
[cite_start]**Goal:** Implement the `move(board, player, remain_time)` function for the Vietnamese board game "Cờ Gánh" to achieve a high win rate in a tournament[cite: 4, 36].

## 1. Context & API Requirements
* [cite_start]**Target Function:** `def move(board, player, remain_time)` [cite: 6]
* [cite_start]**Inputs:** * `board`: 5x5 list of lists (1: 'O', -1: 'X', 0: empty)[cite: 7].
    * [cite_start]`player`: 1 or -1[cite: 22].
    * [cite_start]`remain_time`: float, starting at 99.0s[cite: 23].
* [cite_start]**Output:** Tuple `((start_row, start_col), (end_row, end_col))`[cite: 24, 25, 26].
* **Constraint:** STRICT TIME LIMIT. [cite_start]The function must return within < 2.9 seconds per move to avoid timeout failure (Limit is 3.0s)[cite: 30].

## 2. Core Strategy (Strict Adherence)
DO NOT write logic to simulate moves (Gánh, Chẹt, Mở) from scratch. You MUST import and use the exact game logic functions already provided in `a2_260408.py`:
* `copy_board(board)`
* `get_valid_moves(board, player)` -> To get tree branches.
* `act_moves(move, player, board)` -> To mutate the copied board to the next state. Note: this function returns a list `mo` (forced moves), handle it carefully.

## 3. Algorithm: Minimax with Alpha-Beta Pruning + Iterative Deepening
1.  [cite_start]**Iterative Deepening Search (IDS):** This is mandatory to manage the 3-second time limit[cite: 30]. Start searching at `depth = 1` and increase. Use `time.time()` to track elapsed time. If elapsed time > 2.8 seconds, abort the search and immediately return the `best_move` found in the last fully completed depth.
2.  **Alpha-Beta Pruning:** Standard implementation to prune useless branches.
3.  **Move Ordering (Optimization):** Sort valid moves before expanding nodes. Prioritize moves that result in capturing enemy pieces (you can do a shallow check) to trigger Alpha-Beta cutoffs earlier.

## 4. Heuristic Evaluation Function
Write a highly optimized `evaluate_state(board, player)` function. It should return a high positive score for advantageous states for `player`.
Consider these weights (adjust as you see fit for optimal play):
* **Piece Difference (High Weight):** Count of `player` pieces minus count of `enemy` pieces. This is the most crucial metric.
* **Positional Advantage (Medium Weight):** The center `(2,2)` is critical. Edges and corners have different strategic values. Assign static weights to the 5x5 grid positions.
* **Mobility (Low Weight):** Number of `valid_moves` available for the player vs the enemy. Being blocked (Chẹt) means 0 mobility.

## 5. Output Request
Generate the complete Python code containing the `move` function and all necessary custom helper functions (like `minimax`, `evaluate_state`). Keep the code clean, modular, and extremely performant. [cite_start]Focus on processing speed to reach higher depths within the 3-second window[cite: 30].