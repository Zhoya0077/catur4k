# catur4k — a chess engine in under 4 KB of Python

Entry for the make.fun competition **tiny-chess-01**: the strongest chess
engine that fits in a single `main.py` of at most 4,096 bytes.

The engine is a 120-square mailbox alpha-beta search written from scratch:

- negamax with principal-variation search and a transposition table
  (seeded Zobrist hashing, fully deterministic — no clock, no randomness
  beyond the fixed seed baked into the tables)
- null-move pruning and quiescence over captures, promotions and check
  replies, with in-check positions handed back to the main search
- move ordering by transposition-table move first, then MVV-LVA captures
- evaluation = material plus piece-square tables for every piece, the king
  table included, packed as one arithmetic-coded string
- iterative deepening under a fixed 55,000-node budget, so a move costs
  roughly two to three seconds and always the same move on replay

`main.py` is the packed form of the readable source `engine.py`, produced
by the competition kit's own `makefun compress`. The source stays in this
folder next to the entry; `main.py` never reads it at run time.

## Run it

The engine speaks the competition's environment protocol: point
`MAKEFUN_ENV_URL` at a running game environment and start the bot —

```sh
MAKEFUN_ENV_URL=http://localhost:PORT python3 main.py
```

It asks the environment for the next game, answers every `your_move` with
one UCI move, and exits when the trial reports `done`. Python 3.13,
standard library only.

## Verification

Move generation is perft-verified (start position depth 3 = 8,902;
Kiwipete depth 2 = 2,039) and the packed entry plays the practice gauntlet
deterministically: the same position and game history always produce the
same move, byte for byte on replay.

Entered on make.fun as @a-11994 for
[tiny-chess-01](https://make.fun/bounty/tiny-chess-01) — referral
[n8hc0qc9](https://make.fun/r/n8hc0qc9). MIT licensed.
