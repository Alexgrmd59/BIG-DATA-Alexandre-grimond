# W04 observations

## Task 1

I set five bit positions for each item and never clear them, so anything I added always stays a possible match. With 800 items, Bloom predicted 0.860% false positives and I measured 0.835%; the small gap is just sampling noise from the 20,000 test queries.
For Flajolet-Martin, I averaged the estimates in pairs and took the median: 24,576 estimated vs 19,953 distinct items. A plain mean shot up to 73,856 because of one large result, while a plain median gave 16,384.
The reservoir holds only k items. The running count sets the random replacement range, so the sample stays uniform even though I do not know when the stream ends.

## Task 2

At 25 million items, the exact set took 83.32 s and peaked at 710.327 MiB. I stopped because of the wait, not because the 32 GB of RAM ran out.
Across the paired 100,000 to 1.6 million runs, exact memory grew about 11.9x for 16x more items, while FM stayed near 5.13 KiB. That is roughly linear growth for the set and flat memory for FM.
FM's ratios bounced between 1.23x and 1.62x. A factor-of-two estimate is fine for a rough daily visitor count, but not for billing or any report that needs an exact unique-user number.

## Task 3

Minimizing `p(k) = (1 - exp(-kn/m))^k` sets `exp(-kn/m) = 1/2`. With 10 bits per item, that gives `k = (m/n) * ln(2) = 6.93`, so I used 7 hash positions.
The predicted minimum is about 0.82%. The benchmark got 0.844% (1,687 out of 200,000), just above that floor, down from 9.511% for the baseline, with no false negatives and the same 80,000-bit budget.
If the stream length were unknown, I would track insertions and start a new filter when the first reaches its planned capacity. Guessing too low raises false positives; guessing too high wastes memory.
