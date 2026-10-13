# Task 2: how fast does PageRank settle?

I ran the fixed-seed graph on my Windows 11 laptop (AMD Ryzen AI 9 HX 370, 32 GB of RAM, Python 3.14.3). Codex and the usual background apps were open. The times below cover PageRank, not graph generation. They are single runs, so small timing differences can be noise.

## Changing beta

These runs used 1,200 pages and `tol = 1e-10`.

| Beta | Iterations | Time (s) |
|---:|---:|---:|
| 0.50 | 14 | 0.032 |
| 0.70 | 17 | 0.043 |
| 0.85 | 20 | 0.050 |
| 0.95 | 23 | 0.103 |
| 0.99 | 24 | 0.057 |

The count goes up as beta gets close to 1. With fewer random jumps, rank keeps following links and takes longer to settle. The time does not rise perfectly with it because these short runs are noisy.

The top-10 **order** first changes at beta 0.70: `p00004` moves ahead of `p00000` in positions 7 and 8. The same ten pages stay in the list at every tested beta, and the top three keep their order. So the main winners look stable here, but the exact published order still depends on beta.

## Changing graph size

Before running this, I expected the bigger graph to take longer because each turn would process more pages and links. I did not expect it to need more turns just because it had more pages; the link structure also matters. Both generated graphs contain dead ends and a small trap.

These runs used `tol = 1e-10`. The 20,000-page graph is 16.7 times larger than the 1,200-page graph.

| Pages | Beta | Iterations | Time (s) |
|---:|---:|---:|---:|
| 1,200 | 0.85 | 20 | 0.050 |
| 20,000 | 0.85 | 21 | 1.239 |
| 1,200 | 0.95 | 23 | 0.103 |
| 20,000 | 0.95 | 24 | 1.487 |

At beta 0.85, the larger graph took only one extra iteration but about 25 times longer. Each iteration has more pages and links to process; graph size alone does not decide how many iterations it needs.

## Changing tolerance

These runs used 1,200 pages and beta 0.85.

| Tolerance | Iterations | Time (s) |
|---:|---:|---:|
| `1e-3` | 7 | 0.018 |
| `1e-6` | 12 | 0.027 |
| `1e-10` | 20 | 0.050 |

From `1e-3` to `1e-6`, three extra decimal digits cost five iterations. From `1e-6` to `1e-10`, four more digits cost eight iterations. That is roughly two extra iterations per digit on this graph.
