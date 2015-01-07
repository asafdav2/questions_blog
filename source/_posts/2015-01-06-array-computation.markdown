---
layout: post
title: "array computation"
date: 2015-01-06 07:40:49 +0200
comments: true
published: false
categories: [Bitwise operations, C] 
---

**Question**: Implement the following code without using multiplication / power / modulus operators:

``` C
    int A[ARRAY_SIZE];

    for (int i = 0; i != ARRAY_SIZE; ++i) {
      A[I] = pow(6, i) + 5*i + i % 4;
    }
```
<!--more-->
---

**Solution**:

The following is a C solution:

``` C
    int A[ARRAY_SIZE];
    int a = 1;

    for (int i = 0; i != ARRAY_SIZE; ++i) {
      A[I] = a + ((i << 2) + i) + (i & 3);
      a = (a << 2) + a + a;
    }
```


