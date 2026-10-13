# Task 2: where does exact counting get painful?

I ran this on my laptop (Windows 11, AMD Ryzen AI 9 HX 370, 32 GB of RAM, Python 3.14.3), plugged in. Codex and the usual background apps were open, with no other heavy programs running. The stream was generated with the fixed seed in `task2_limits.py`. Each method was timed separately, with `tracemalloc` measuring peak Python allocations during its run rather than the whole process's RAM use.

| Stream items (n) | True distinct | Exact time (s) | FM time (s) | Exact peak (MiB) | FM peak (KiB) | FM estimate / true |
|---:|---:|---:|---:|---:|---:|---:|
| 100,000 | 36,702 | 0.32 | 28.81 | 3.744 | 5.126 | 1.228x |
| 200,000 | 73,410 | 0.57 | 58.15 | 5.494 | 5.126 | 1.618x |
| 400,000 | 146,970 | 1.26 | 116.57 | 11.054 | 5.128 | 1.226x |
| 1,600,000 | 587,625 | 4.91 | 498.37 | 44.487 | 5.128 | 1.561x |

These four paired runs cover a 16x range. The exact set's peak grew from 3.744 to 44.487 MiB, about 11.9x for 16x more stream items. That is roughly 28.5 extra bytes per added stream item over this range, so memory grows roughly with n. FM stayed between 5.126 and 5.128 KiB, just 2 bytes apart from the first to the last run: its memory is effectively constant because it keeps 64 counters, not the stream.

I also stressed the exact set alone at 25,000,000 items. It found 9,179,304 distinct values in 83.32 s and peaked at 710.327 MiB. From 1.6 million to 25 million items, its memory grew 16.0x for 15.6x more items. I stopped there because waiting over a minute for one exact count was already unpleasant; time gave out first, not RAM. I did not run FM at this stress size because its measured time at 1.6 million predicts roughly two hours.

FM's accuracy did not steadily improve with n: its ratios were 1.228x, 1.618x, 1.226x, then 1.561x. The 64 counters keep memory flat, but they also leave some random error. FM was much slower here: it processes 64 hash values per item, and `tracemalloc` likely adds a lot of overhead to that work.
