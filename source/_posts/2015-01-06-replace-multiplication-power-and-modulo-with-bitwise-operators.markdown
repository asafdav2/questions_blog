---
layout: post
title: "replace multiplication, power and modulo with bitwise operators"
date: 2015-01-06 07:40:49 +0200
comments: true
categories: [Bitwise operations, C] 
---

**Question**: Given the following equation:

$$foo(x) = 6^x + 5x + x \text{ mod } 4$$

Your task is to fill an array $$A$$ (with size $$N$$) such that $$A[i] = foo(i)$$. Your code must not use the multiplication, power or modulo operators.

<!--more-->

---

**Solution**: To replace the power operation, we'll hold a variable ```a``` and multiply it by 6 in each iteration. Since we're not allowed to use the ```*``` operator,
we'll use bitwise operators: left shift of two bits is essentially a multiplication by 4, and we'll add ```a``` to the result twice to get multiplication by 6.
The multiplication by 5 part is very similar. All that remains is to replace the modulo operation. mod by 4 is equal to bitwise AND with 3.

The following is a C solution:

``` C
    int a = 1;

    for (int i = 0; i != N; ++i) {
      A[I] = a + ((i << 2) + i) + (i & 3);
      a = (a << 2) + a + a;
    }
```


