# W07 observations

## Task 1

When a cluster is empty, I restart it at the point furthest from the occupied centroids. Keeping its old centroid would be simpler, but might leave that cluster empty.
Across 40 seeds, random starts found the three blobs 27 times and k-means++ did so 40 times. The extra distances before iteration one are useful when bad random starts would force many reruns.
SSE keeps falling as k grows and can reach zero with one centroid per point, so minimising SSE alone would always pick too many clusters.

## Task 2

With 1,000 points, contrast first dropped below 1 at 50 dimensions and below 0.5 at 200; relative spread fell from 0.476 to 0.041 as distances became more alike.
At 50 dimensions, changing the point count from 100 to 1,000 to 5,000 gave contrasts 0.811, 0.982 and 0.877. The extremes depend on the sample too, so the change was not monotonic.
This does not directly threaten week 3's LSH: it used Jaccard similarity on shingle sets, not Euclidean distance on uniform points. I would check those similarities separately.

## Task 3

I keep each point's cluster label plus an upper bound to its own centroid and a lower bound to the other centroids: two extra distance numbers per point.
The lower bound skipped 549,881 checks and the centroid-separation bound skipped 218,137. Maintaining them needs one update per point and about 66 centroid-pair distances per later iteration, plus 12 movement distances.
Both runs used 43 iterations and got SSE 10,737,882.94 to the displayed precision. Bounds only skip assignments proved unchanged, cutting distance calls by 88.8%; the first iteration used 240,012 of 1,150,998 calls (20.9%).
