---
layout: post
title: "Random number with distribution"
date: 2014-12-11 21:12:24 +0200
comments: true
categories: [Binary Search, Scala]
---

**Question**: Implement a function that takes a list of doubles $$D = [d_0, d_1, \ldots, d_n]$$ (where $$\sum_{i=0}^{n} d_i = 1.0$$) and returns $$i$$ with probability $$d_i$$.
For example, given the list $$[0.3, 0.4, 0.2, 0.1]$$, the function should return 0 with probability 0.3, 1 with probability 0.4, 2 with probability 0.2 and 3 with probability 0.1.

**Solution**:
We'll start by building the running sums list of the input list, $$D'$$. Then, we'd generate a random number $$r \in [0, 1]$$ and find where it 
"falls" in the running sums list; We'll return the index $$j$$ that satisfies $$D'_j \geq r \wedge D'_{j+1} < r$$.
For example, the running sums list for the previous example is $$D'=[0.0, 0.3, 0.7, 0.9, 1.0]$$. Assuming we generated $$ r = 0.67$$, it falls between indices 1 and 2, 
so we'd return 1.
Since the running sums list is ordered by nature, we can use binary search to find the desired index.

The time complexity of the solution is $$O(n)$$ to build the running sums list and $$O(\log{}n)$$ for the search part. If we're told that we'd generate many random numbers
for a given distributions list, we could optimize things by building the running sums list once and reusing it, as seen in the following Scala implementation. After building the
running sums list, it returns a function which reuses it.

``` Scala
    import scala.collection.Searching._

    class Distribution {
      def precomputeFor(distribution: List[Double]): () => Int = {
        val sums = runningSums(distribution, 0.0)
        () => { 
            sums.search(util.Random.nextDouble) match {
              case Found(p) => p
              case InsertionPoint(p) => p + 1
            }
        }
      }
    
      def runningSums(dist: List[Double], sum: Double): List[Double] = 
        if (dist.isEmpty) Nil else sum :: runningSums(dist.tail, sum + dist.head)
    }
```

Have any questions / remarks? leave them at the commonts below!
