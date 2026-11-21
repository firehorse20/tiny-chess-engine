# Tiny Chess Engine

A deterministic 4 KB chess engine for tiny-chess-01. Single file
`main.py` (4,096 bytes, Python 3.13 stdlib only), lzma-packed.

## Search

Negamax alpha-beta with iterative deepening under a fixed node budget
(120,000 nodes, never a clock), so replays are byte-identical.
Quiescence search (captures/promotions; all evasions in check),
transposition table, one killer move per ply, MVV-LVA capture ordering,
and late-move reductions with a full-depth re-search when the reduced
score beats alpha. Deterministic: no wall clock, no unseeded randomness.

## Evaluation

Material + piece-square tables (all pieces, 4-bit packed) with a
tapered king term. 0x88 board, Zobrist hashing, incremental eval.

## Protocol

Plays through `MAKEFUN_ENV_URL`: `POST /game`, `POST /move`. Legal
moves only; falls back to the env's first legal move if needed.

## Practice trials (local, kit env server)

- trial-0: 0 points, Elo 0 (6 losses by checkmate).
- v6 dev reference (unpacked, 24 KB): mean Elo 1305 over 3 trials.

The 4 KB packing cost substantial strength vs the unpacked reference.
