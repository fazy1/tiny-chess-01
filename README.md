# tiny-chess-01

A chess engine in 3,878 bytes of dependency-free Python 3.13. `main.py` is a
compressed payload (lzma + base85) with a one-line unpacker; the engine it
unpacks is a full negamax searcher: 0x88 board with make/unmake, Zobrist
hashing, a transposition table kept warm across moves, PVS with late-move
reductions and null-move pruning, check extensions, quiescence search with
captures-only move generation, check evasions and delta pruning, repetition
detection (game history plus search path), MVV-LVA + killer + history
ordering, and an eval of material, piece-square tables, a bishop-pair bonus
of 50, and an endgame king-advance term. Iterative deepening to a fixed
budget of 100,000 nodes per move. No clock, no randomness: the same
position always gets the same move.

Practice gauntlet (kit env server + scorer): trial-0 Elo 1,396; trial-1
Elo 1,669; trial-2 Elo 1,396; mean Elo 1,487 (14.0/24 points) vs the
house baseline mean of 1,325. Trial walls 948-1,569 s of the 1,800 s
budget. Perft suites (startpos, kiwipete, positions 3-6) all pass; mate
puzzles and determinism (same position twice, same move) verified.

Strengthening round 7 (16 Nov 2026) tried four single-change variants
from this version — a tempo bonus, a heavier killer weight, a
shallower quiescence limit, and a higher null-move material
threshold — and none of them passed the gate (all scored below
1,487 mean Elo). This version remains the strongest one found.

## Run it locally

`grade-local` with the default driver runs it as
`python3 -P -s main.py` against the practice gauntlet through the env at
`MAKEFUN_ENV_URL`. (The process driver is Node-only and refuses Python;
official grading runs Python in Docker.)
