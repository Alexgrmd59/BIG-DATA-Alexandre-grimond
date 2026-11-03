# Task 2: when do distances lose contrast?

I sampled about 20,000 random point pairs per run on my Windows 11 laptop (AMD Ryzen AI 9 HX 370, 32 GB of RAM, Python 3.14.3). Codex, VS Code and the usual background apps were open. Each time includes making the points and measuring their distances.

With 1,000 points, the results were:

| Dimensions | Mean distance | Minimum | Maximum | Contrast | Relative stdev | Time (s) |
|---:|---:|---:|---:|---:|---:|---:|
| 2 | 0.521 | 0.003 | 1.326 | 488.582 | 0.476 | 0.047 |
| 5 | 0.884 | 0.111 | 1.687 | 14.257 | 0.282 | 0.055 |
| 10 | 1.274 | 0.438 | 2.159 | 3.925 | 0.193 | 0.082 |
| 20 | 1.815 | 0.821 | 2.670 | 2.253 | 0.135 | 0.114 |
| 50 | 2.867 | 1.909 | 3.785 | 0.982 | 0.083 | 0.247 |
| 100 | 4.068 | 3.206 | 4.954 | 0.545 | 0.060 | 0.450 |
| 200 | 5.776 | 4.871 | 6.682 | 0.372 | 0.041 | 0.912 |

Here `contrast = (maximum - minimum) / minimum`. It first fell below 1 at about 50 dimensions and below 0.5 at about 200. The minimum at two dimensions was very close to zero, which makes the contrast of 488.582 look dramatic; an extreme pair can make this measure noisy.

Mean distance grew roughly with the **square root** of dimension, not linearly. From 50 to 200 dimensions, dimension grew 4x and mean distance grew 2.01x. For uniform coordinates, each squared coordinate difference averages 1/6, so a typical distance is about `sqrt(d/6)`; at 200 dimensions that predicts 5.774, close to the measured 5.776.

Relative standard deviation fell from 0.476 to 0.041. I think it better describes how *typical* distances become alike, while contrast looks only at the most extreme sampled pair. Both show distance concentration, but contrast depends more on sample size and outliers.

At a fixed 50 dimensions, changing the number of points gave:

| Points | Contrast | Mean distance | Relative stdev |
|---:|---:|---:|---:|
| 100 | 0.811 | 2.864 | 0.081 |
| 1,000 | 0.982 | 2.867 | 0.083 |
| 5,000 | 0.877 | 2.870 | 0.084 |

Point count affected the observed extremes, but not in a smooth order in these single runs. The script samples only about 20,000 pairs each time, so a different set of points can change the minimum and maximum. The largest run here was 200 dimensions with 1,000 points; it took 0.912 s.

This result does **not** directly invalidate week 3's LSH: that lab used Jaccard similarity of shingle sets, not Euclidean distance between uniform points. High-dimensional data can still make similarities less useful, but we would need to inspect the actual Jaccard distribution before saying that happened there.
