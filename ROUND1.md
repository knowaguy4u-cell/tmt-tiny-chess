Round 1 — tiny-chess-01

Artifact: main.py, 3,266 bytes (830 bytes below 4,096).
SHA-256: c96cf49effcb607b49ce06e206ad9cdf7ca89481359929fe0a513bf850415551

Design

Original engine authored in this session; MIT starter seeded the entry.
One standard-library Python file, packed with the kit's makefun compress
(LZMA plus Latin-1 loader). The readable source is ../build/engine.py;
it is development material, excluded from the entry snapshot. Runtime
never reads it. All search, evaluation, data and HTTP driver are embedded.

Signed 0x88 board; legal moves include castling, en passant and all four
promotions, with king-safety filtering. Iterative-deepening negamax/PVS,
alpha-beta, depth/bound/mate-distance transposition entries, check-aware
quiescence, null-move pruning and late-move reductions. Move ordering uses
TT moves, captures/promotions, killers and history. Search has a fixed
36,000-node budget, retaining the last completed iteration; forced moves
return immediately. No clock or randomness influences a move.

Evaluation combines material with generated tapered piece-square tables,
passed/doubled/isolated pawns, bishop pair, king centralization in endings
and pawn shelter. Pawn and king terms are cached. Phase is fixed per root.
Search handles repetition and the fifty-move threshold; repetition is
conservatively scored drawn on a repeated search/history position.

Verification

- Perft: start position depth 4 = 197,281; Kiwipete depth 3 = 97,862;
  rook/pawn en-passant test depth 4 = 43,238. All matched known totals.
- Differential checks against the kit's Position rules: 1,920 positions
  across 12 seeded legal walks; exact legal-move sets, successor boards,
  sides, castling rights and halfmove clocks. Incremental PST scores
  matched full recomputation for every tested successor.
- Mate-in-one, mating promotion and hanging-queen tactical checks passed.
- Final packed engine self-play at 3,000 nodes: 68 plies, repetition draw;
  101 plies, checkmate after promotion. Every move was accepted by the
  kit's Game rules; no crashes. Both games exactly reproduced the build
  before the evaluation-cache optimization.
- Packed payload decompresses exactly to the readable source.

Practice trial 0: kit scorer Elo 1451; 4 wins, 0 draws, 2 losses;
spare_plies 394. Beat depth-2/3/4 and nodes-100k; lost to nodes-250k/500k.
325 entry moves; 762.20 s total; request-cycle mean 2.34 s, max 6.40 s
(conservative timings include the house reply).
No illegal requests, time losses, abandoned games or crashes.
Full replay of the final cache-optimized build is in progress.

Limits and handoff

Docker daemon access and listening sockets are denied here; only Python
3.12.3 and Node 24.22.1 are available. Tests use the kit's unchanged
Gauntlet/Game/Position code with a socket-free adapter at urllib's
Request/urlopen boundary, and its unchanged scorer. This is not an
end-to-end grade-local Docker run. Python 3.13, real HTTP transport,
production timing, remaining practice instances, final eight-game trials,
and superiority to the Sunfish baseline remain unproven.

No submission, account setup, wallet/credential access, model call or
external network access was performed. Transcript/intake checks and
repository ownership/freshness/public metadata are not certified. Preserve the original session log and readable source for authorship.
