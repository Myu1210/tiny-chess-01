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

Revision 2: quiescence search now uses delta pruning (margin 150): a
capture whose stand-pat score plus the captured piece's value plus the
margin still cannot reach alpha is skipped. This spends the node budget
on captures that can matter instead of hopeless ones — about 20–30%
fewer nodes per depth, which is worth roughly +150 Elo in local
self-play against revision 1 at 5,000 nodes/move (trial-0 openings
11W 2D 3L, trial-1 openings 10W 1D 5L). The change is paid for by
golfing the source (constant folding, arithmetic instead of ternaries
for pawn/castling code, killer-table alias); the root mate early-exit
was removed, so a found mate is now refined to the fastest mate
within the node budget instead of stopping at the first mate score.
The 45,000-node budget is unchanged.

Local practice (kit scorer): trial-0 Elo 1253, 2 wins, 2 draws, 4 losses;
trial-1 Elo 1087, 2 wins, 0 draws, 6 losses (revision 1). Revision 2
official practice trials (kit env + scorer, same draws): trial-0 Elo
1253 (2W 2D 4L), trial-1 Elo 1087 (2W 0D 6L), trial-2 Elo 1396
(4W 0D 4L, including a win over the nodes-500k rung). Three-trial
mean 1245 vs revision 1's 1171 (1253/1087/1173, trial-2 measured the
same way for the comparison); no regression on any trial. Trial-2 was
re-run and reproduced byte-identically (deterministic replay). The
house baseline's practice mean is 1325, so revision 2 is still below
prize level — but it is strictly the stronger entry.

Rejected in revision-2 testing (all measured locally at 5,000
nodes/move, 16 games vs revision 1, trial-0 openings): LMR (0 Elo),
king pawn-shield eval (−22), stronger piece-square scaling (−22),
null-move pruning (−44), QS delta margin 300 (−66), margin 100 (+163)
and margin 200 (+163) — margin 150 (+191) was best. A 65,000-node
budget was tried in revision 1 and rejected (trial-0 Elo 1087 with a
game-1 time loss).

## Layout

- `main.py` — the whole engine (4,085 bytes, packed).
- `LICENSE` — MIT.
