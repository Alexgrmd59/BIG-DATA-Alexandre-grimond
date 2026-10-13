# W05 observations

## Task 1

In the broken version, a dead end's rank disappears; I spread it evenly across all pages instead.
Letting the surfer jump to any page also gets it out of the C-D spider trap, so the same idea fixes both problems.
With beta = 0.85, the surfer follows a link 85% of the time when there is one, and jumps to a random page the other 15%.

## Task 2

As beta rose from 0.50 to 0.99, convergence went from 14 to 24 iterations because fewer random jumps let rank circulate longer.
Going from 1,200 to 20,000 pages at beta 0.85 added just one iteration, but time went from 0.050 s to 1.239 s because each turn had more work.
The top-10 order first changed at beta 0.70 (positions 7 and 8 swapped), although the same ten pages stayed in it. Even a fairly stable ranking depends on the chosen beta.

## Task 3

I reused the graph's link lists and kept two rank maps, about `2n + 6` floats at peak, instead of an `n × n` matrix. The given link lists hold the `E` edges.
Teleportation and dead-end rank make one shared starting amount for every page each turn. Filling `n` scores does not need an `n × n` matrix.
The worst rank difference was `1.17e-15`, just floating-point addition order. My version used 2,406 floats instead of 1,440,000 and ran about 56 times faster in this benchmark.

## Task 4 (optional)

At 500,000 pages, my local version took 69.02 s and traced 172.2 MB of extra Python allocations. Waiting was the first practical limit; the edge list was built before that memory measurement.
I could not measure Spark here because Java and PySpark are missing. In the given Spark code, pages with no incoming contribution disappear, so teleportation is not added to every page and the rank total can shrink.
