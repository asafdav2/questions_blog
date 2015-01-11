---
layout: post
title: "plus minus array"
date: 2014-12-01 23:17:39 +0200
comments: true
categories: [Binary Search, Scala]
---

**Question**: We are givan an array $$A$$ (with size $$n$$) where every cell holds either '+' or '-'.
We're told that $$A[0]$$ = '+' and that $$A[n-1]$$ = '-'. Describe an algorithm to find an index $$j$$ such that $$A[j]$$ = '+'
and $$A[j+1]$$ = '-'.

**Example**: 

|A |+|+|-|+|+|+|-|-|+|-|
|id|0|1|2|3|4|5|6|7|8|9|

Correct answers for this example are $$j=1$$, $$j=5$$ or $$j=8$$ (each one is acceptable, no need to return them all).
<!--more-->

---

**Solution**: We can use logic similar to binary search to solve this in $$O(\log{} n)$$ time complexity. We'll use two 
variables, $$l$$ and $$h$$, and maintain the following invariant: $$A[l]$$ = '+' and $$A[r]$$ = '-'. In each step
we'll check if $$r=l+1$$. If so, we're done. else, we find the center point $$c=\frac{l+r}{2}$$. If $$A[c$$] = '+' we'll
update $$l=c$$, else $$r=c$$. Since in each step the distance between $$l$$ and $$r$$ diminishes, we know the process
will eventually terminate.

The following is a Scala implementation:


``` Scala
    class PlusMinusArray {
    def search(array: Array[Char]): Int = {
      def searchInner(l: Int, h: Int): Int = {
        if (h == l + 1) l else {
          val mid: Int = l + (h - l) / 2
          array(mid) match {
            case '+' => searchInner(mid, h) 
            case '-' => searchInner(l, mid)
            case _   => throw new IllegalArgumentException("invalid character " + array(mid)) 
          }
        }
      }
    
      searchInner(0, array.size - 1)
    }
    }
```
Have any questions / remarks? leave them at the commonts below!
