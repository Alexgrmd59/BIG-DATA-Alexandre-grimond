# Task 2: when does LSH become faster?

I ran the script on my laptop (Windows 11, AMD Ryzen AI 9 HX 370, 32 GB of RAM, Python 3.12.14). Codex and the terminal were open, but no other heavy programs were running. For each size, the script made exactly that many documents using the same shingle settings as the benchmark. Making the documents was not included in the times below.

| Documents (n) | Brute time (s) | LSH time (s) | Brute comparisons | LSH comparisons | Brute peak (MiB) | LSH peak (MiB) |
|---:|---:|---:|---:|---:|---:|---:|
| 250 | 0.39 | 2.62 | 31,125 | 15 | 0.008 | 0.647 |
| 500 | 1.52 | 5.01 | 124,750 | 32 | 0.011 | 1.296 |
| 1,000 | 5.70 | 9.95 | 499,500 | 76 | 0.012 | 2.558 |
| 1,500 | 12.21 | 14.66 | 1,124,250 | 120 | 0.024 | 3.937 |
| 1,750 | 17.85 | 17.85 | 1,530,375 | 145 | 0.025 | 4.551 |
| 2,000 | 22.76 | 18.96 | 1,999,000 | 170 | 0.027 | 5.191 |
| 4,000 | 91.33 | 38.80 | 7,998,000 | 467 | 0.040 | 10.407 |

I tested seven sizes from 250 to 4,000 documents, so the range is 16x. When I doubled n, brute-force time went up by 3.92x (250 to 500), 3.76x (500 to 1,000), 3.99x (1,000 to 2,000), and 4.01x (2,000 to 4,000). These are close to 4x, so the measured time grows roughly like n².

At 250 documents, brute force took 0.39 s and LSH took 2.62 s. LSH first has to build 60-value signatures and group 20 bands, so that setup takes more time than checking all pairs at small sizes. Brute force was still faster at 1,500. At 1,750, both took about 17.85 s, and by 2,000 LSH was faster. I would put the crossover around 1,750 documents, rather than claim an exact point from one run.

At 4,000 documents, brute force took 91.33 s. Waiting time was the first problem; I did not see memory pressure. The script measured peaks of 0.040 MiB for brute force and 10.407 MiB for LSH. These `tracemalloc` numbers cover extra Python allocations during each method call, not the input documents or the program's total RAM use.
