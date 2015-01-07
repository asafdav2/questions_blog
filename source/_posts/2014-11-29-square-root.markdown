---
layout: post
title: "Square Root (Solution)"
date: 2014-11-29 18:20:21 +0200
comments: true
categories: [Binary Search, Scala] 
---

**Question**: Givan two real numbers $$x$$ and $$\epsilon$$, find the square root of $$x$$ within error margin of $$\epsilon$$ 
(in other words, find $$y$$ such that $$ \lvert \sqrt{x} - y \rvert < \epsilon$$). Obviously, you're not allowed to use the standard ```sqrt``` function. 
<!--more-->
---

**Solution**: The general idea is to guess a solution $$y$$ and then iteratively improve it. If $$y^2 > x$$, we should aim lower, so in the next step we'll try $$y - stepSize$$. 
else, we should aim higher, so we'll try $$y + stepSize$$. As we approach our target we should refine our search, so we'll divide $$stepSize$$ by 2.
We can start from any number, we'll select arbitrarily $$\frac{x}{2}$$.

The following is a Scala implementation:

``` Scala
    class Sqrt {
      def sqrt(x : Double, err: Double): Double = sqrtHelper(x, err, 0.5 * x, 0.25 * x)
    
      def sqrtHelper(x: Double, allowedError: Double, curr: Double, stepSize: Double): Double = {
        val err = x - (curr * curr)
        if (Math.abs(err) <= allowedError)
          curr
        else
          sqrtHelper(x, allowedError, if (err > 0) curr + stepSize else curr - stepSize, 0.5 * stepSize)
      }
    }
```

Have any questions / remarks? leave them at the commonts below!
