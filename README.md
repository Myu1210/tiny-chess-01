# tiny-chess-01

A chess engine in 4,096 bytes of Python (packed with lzma + base85).

0x88 board, legal move generation (castling, en passant, promotion),
iterative-deepening alpha-beta with a transposition table, killer moves
and MVV-LVA ordering, quiescence over captures/promotions/check evasions,
and an evaluation of material plus formula piece-square terms, bishop
pair (+50) and phase-interpolated king tables. Repetitions against the
game's history score as draws. Search runs to a fixed 45,000-node budget
per move (depth 4 in the middlegame, depth 5 from the start), never a
clock, so replays match.

Local practice (kit scorer): trial-0 Elo 1253, 2 wins, 2 draws, 4 losses;
trial-1 Elo 1087, 2 wins, 0 draws, 6 losses. Revision: bishop-pair bonus
30 -> 50; a 65,000-node budget was tried and rejected (trial-0 Elo 1087
with a game-1 time loss).

## Layout

- `main.py` — the whole engine (4,065 bytes, packed).
- `LICENSE` — MIT.
