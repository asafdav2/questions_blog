---
layout: post
title: "Deadly Island"
date: 2014-11-28 06:57:33 +0200
comments: true
categories: [Dynamic Programming, Scala]
---

**Question**: Imagine an island made of $$N \times M$$ tiled board, with a man standing in some initial position $$(x,y)$$. In each turn, the man steps either up, down, left or right (each
with probability 0.25). If the man steps out of the island, he dies. Calculate the probability that the man will live after traveling for $$n$$ steps.

---

**Solution**: We'll mark the probabily that the man, located at $$(x,y)$$ will live for $$n$$ steps with $$p(x,y,n)$$. Using the [Law of total probability](http://en.wikipedia.org/wiki/Law_of_total_probability),
we can derive the following equation: 

$$
\begin{split}
&p(x,y,n) = \\ &\frac{p(x-1,y,n-1) + p(x+1,y,n-1) + p(x,y-1,n-1) + p(x,y+1,n-1)}{4}
\end{split}
$$ 

Of course, if the man steps outside of the island boundaries, the probability is 0.
 
This is easy enough to program, but to make it run at reasonable speed, we'll memoize the probabilities calculated before for certain values of $$x,y$$ and $$n$$ (a common idiom 
in [Dynamic Programming](http://en.wikipedia.org/wiki/Dynamic_programming).
The following is a Scala implementation:

``` Scala
class DeadlyIsland(n: Int, m: Int) {

  val MaxSteps = 100 

  val memoization = Array.ofDim[Double](n, m, MaxSteps)

  def calculate(x: Int, y: Int, steps: Int): Double = {
    if (x < 0 || x >= n || y < 0 || y >= m)
      0.0
    else 
      if (steps == 0)
        1
      else 
        if (memoization(x)(y)(steps) > 0.0)
            memoization(x)(y)(steps)
        else {
            val prob = 0.25 * (
                calculate(x - 1, y, steps - 1) +
                calculate(x + 1, y, steps - 1) +
                calculate(x, y - 1, steps - 1) +
                calculate(x, y + 1, steps - 1)
            )
            memoization(x)(y)(steps) = prob
            prob
        }
  }
}
```
Have any questions / remarks? leave them at the commonts below!
 
