---
layout: post
title: "Check for duplicated value in array"
date: 2015-01-06 07:51:24 +0200
comments: true
categories: 
---

**Question**: Given an N-sized array A where all values are in the range [0, M], and given M >> N,
implement a time-efficient algorithm to check if A contains a duplicated value.

**Solution**: Notice that the questions specifically asks to optimize running time, event at the cost of storage.
We'll allocate a new array B with size M, but instead of zeroing all elements (which will take O(M) time), we'll traverse A and zero
B[A[i]] for all 0 <= i < N. We'll then traverse A once again; if B[A[i]] == 0, we'll set it to 1. if it's already 1, we've found a
duplicated item. If we reached the end, no duplicated item exist.

The following is a C solution:

``` C
    bool containsDuplication(int A[]) {
      int B[M];

      for (int i = 0; i != N; ++i) {
        B[A[i]] = 0;
      }
      for (int i = 0; i != N; ++i) {
        if (B[A[i]] == 1) {
          return true;
        }
        B[A[i]] = 1;
      }
      return false;
    }
```


