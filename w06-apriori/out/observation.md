# W06 observations

## Task 1

At support 2, `cola → milk` is the highest-confidence rule I found: confidence 1.0 and lift 1.4, against only 0.4 confidence for `milk → cola`. That direction is worth checking, but two cola baskets are too few to trust it much.
At support 3, `bread → milk` reaches 0.8 confidence, but its lift is only 1.12 because milk is common anyway.
With 2,000 items, brute force allows 1,999,000 pairs. Pass one kept 1,849 items, leaving 1,708,476 possible pairs; the plain run actually held 820,259 observed pair counters.

## Task 2

On 20,000 baskets, even support 1 still ran (7.19 s, 334.2 MB traced). In a separate 200,000-basket stress run at support 1, the 58.62 s wait became my practical limit before RAM did.
From support 400 to 50, the counter count grew about 77x, much faster than the 26x growth in frequent pairs. Later the counters levelled off at 893,456 because that was every pair present in these baskets.
At support 50, I counted 820,259 pairs to get only 6,397 answers. Asking for a slightly lower threshold can mean a lot of extra counting for relatively few useful results.

## Task 3

I used 262,144 hash buckets: the counting array costs about 1.05 MB and its bitmap about 33 KB. PCY kept the same 6,397 answers while cutting peak pair counters from 820,259 to 52,006 (93.7%), including the brief overlap with the output.
Counting the bucket memory too, traced peak allocations were 7.35 MB for PCY against 107.75 MB for plain A-Priori at 20,000 baskets. The crossover was roughly between 2,000 and 5,000 baskets here; it depends on the fixed bucket cost, collisions and how many distinct pairs occur.
With only 65,536 buckets, peak pair counters rose to 290,921. At 16,384 buckets they reached 826,656: collisions made the filter useless.

## Task 4 (optional)

Every chunk uses its share of the global support. If a globally frequent pair were below that limit in every chunk, its total count would also be below the global support, so SON cannot miss it.
With 4, 8 and 16 chunks I got 9,876, 14,201 and 24,418 candidates, but always the same 6,397 final pairs and zero missing. Extra candidates are fine because pass two removes them; scanning observed pairs kept local SON near 2–3 s.
SON is useful when baskets must be split across machines, even with extra overhead here. Spark could not start because PySpark and Java are unavailable on this PC.
