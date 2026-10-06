# W03 observations

## Task 1

I go through the rows once instead of reading the matrix again for each document. That matters when the matrix is too large to keep in memory.
If a signature cannot be split into equal bands, my function raises `ValueError` rather than silently dropping rows.
S1 and S4 get the same two-value signature, so MinHash estimates 1.0 even though their Jaccard similarity is 2/3. More hashes would usually make the estimate less noisy, but would take more time and memory.

## Task 2

On my AMD Ryzen AI 9 HX 370 laptop with 32 GB of RAM and no other heavy programs running, brute force and LSH took about the same time at 1,750 documents.
Doubling n made brute-force time about 3.76x to 4.01x longer, close to the 4x expected for quadratic growth.
At 4,000 documents, brute force took 91.33 s, so time was the first limit. The script measured peak Python allocations of 0.040 MiB for brute force and 10.407 MiB for LSH.

## Task 3

I chose 60 hashes and 20 bands, which makes 3 rows per band. The S-curve step is `(1/20)^(1/3) = 0.368`, below the 0.6 similarity threshold. This should help catch real pairs: at similarity 0.6, `1 - (1 - 0.6^3)^20 = 0.992` is the theoretical chance of becoming a candidate.
With this setting, the benchmark found all 121 true pairs using 200 similarity calls instead of 2,246,140. That is 99.99% fewer comparisons.
I also tried 10 bands of 6 rows. The step moved to `(1/10)^(1/6) = 0.681`, and the candidate chance at 0.6 dropped to 0.380. Calls dropped to 102, but I found only 102 of the 121 true pairs (84.3% recall), below the required 90%.
The score counts similarity calls but not the time to build signatures. With a much larger vocabulary or more hash functions, that work could become a real part of the cost.
