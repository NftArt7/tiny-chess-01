# tiny-chess-01 — golf13 "The Brawler"

A 4 KB chess engine (packed `main.py`, 3,279 bytes of the 4,096 limit).

- 0x88 board, alpha-beta + quiescence (captures-only when not in check)
- Iterative deepening to a fixed 60,000 nodes/move (deterministic replay)
- Transposition table, LMR, null-move (R=3), killer moves
- MVV-LVA + promotion move ordering, piece-square tables
- Castling (incl. check-through rules), en passant
- Draws left to the environment's own adjudication

## Character: aggressive, not generic

golf12 played quiet, centralising chess. golf13 ("The Brawler") plays
*its* games — pawn storms on both wings, pieces lunging forward,
queen sorties, and no sterile draws:

1. **Steeper pawn table** — a pawn on the 7th is worth +80 PST (was
   +50): pawns storm forward instead of sitting.
2. **Forward-leaning piece tables** — knights, bishops, queen and
   rooks gain per rank advanced toward the enemy (N +5, B/Q +4,
   R +2 per rank).
3. **Tempo bonus +12** — the side to move is always a little ahead,
   so it prefers keeping the initiative over shuffling.
4. **Draw contempt −25** — stalemate scores −25 for the side to move
   instead of 0: it fights on in level positions instead of settling.

A real practice-trial win (game 3 vs the depth-4 anchor, 1,199):
`1.e4 e5 2.Nf3 Nc6 3.Bc4 Bc5 4.c3 Nf6 5.b4! Bd6 6.b5 Na5 7.Qa4 Nxc4
8.Qxc4 a6 9.O-O axb5 10.Qxb5 O-O 11.a4` — an Evans-gambit pawn sac
with an early queen sortie — `...24.g3 Bc7 25.g4 f5 26.g5 h6
27.gxh6 gxh6 28.Bc5 Bf4 29.Na3 Qg6 30.Kh1 Qe6 31.Qa8 Be8 32.h4`
— the pawn storm rolls on — `...42.h5 Bd3 43.Rg7+ ... Qg7#`.
All 8 trial games ended in checkmate; none drawn.

## Trial 0 (practice, fresh kit, harness@5ea7544b)

**4 won, 0 drawn, 4 lost — Elo 1,396** (wall 764 s of 1,800 s).
Beats depth-2 (855), depth-3 (1,073), depth-4 (1,199) and nodes-100k
(1,392); loses to nodes-250k (1,497) and above. Up from golf12's
1,326 (3W 1D 4L) on the same trial — the aggression gained Elo as
well as character. A/B self-play vs golf12 at ND=8,000: 4W 5D 3L (+29).

## Verification

- Perft ALL OK (20/20, 6 standard positions); eval 0 mismatches
  over 50 random games (tempo-aware check).
- Packed `main.py` and source choose the same root move at startpos
  (b1c3, 60,000 nodes).
- Snapshot 7,036 / 8,192 bytes.

## Layout

- `main.py` — packed engine (kit `makefun compress` of golf13.py)
- `LICENSE` — MIT (`SPDX-License-Identifier: MIT` first line)
- `README.md` — this file

Source: `~/workspace/makefun/dev/golf13.py` (7,050 bytes, packed by
the kit's `makefun compress`).
