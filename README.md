# tiny-chess

Tiny chess engine in 4,096 bytes of Python (stdlib only) for the make.fun
tiny-chess-01 competition.

## How it plays

- 0x88 board, negamax + alpha-beta, iterative deepening to a fixed node
  budget (100,000 nodes per move) — deterministic, no clock, no randomness.
- Transposition table, killer moves, MVV-LVA capture ordering.
- Quiescence over captures, promotions and check evasions.
- Incremental eval: material + formula piece-square tables, bishop pair,
  king middlegame/endgame interpolation.
- Deliberately no null-move pruning and no late-move reductions: at these
  shallow depths the prunings cost more tactics than the extra depth
  gains (measured: pure alpha-beta outscores the pruned version by a
  wide margin on the kit's practice trials).

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed,
  Python 3.13, standard library only. It reads, imports or execs no other
  file in the repository.
- The source is written readable (`engine_src.py`, kept out of the repo),
  then packed with `makefun compress` into `main.py` (lzma payload).
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes in
  all, and no file over 4,096. MIT licence.

## Practice Elo

Measured with the kit's manual trial runner (1 CPU, taskset):

- trial 0: 1,809 (5W 1D 0L)
- trial 1: 1,191 (2W 1D 3L)
- trial 2: 1,451 (3W 2D 1L)
- mean: 1,484

Practice floor to enter the final is 1,154. The house baseline
(sunfish) rates ~1,365 mean on the same 3 practice trials.
