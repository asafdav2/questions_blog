---
layout: post
title: "Sort words by length and frequency"
date: 2014-12-03 19:32:20 +0200
comments: true
categories: [Scala]
---

**Question**: Implement a function that takes an unordered list of words (assume words may apper more than once), 
and returns a list of distinct words (that is, without duplications), sorted first by word 
length (descending) and then by the number of occurrences in the input list (descending as well).

**Example**: 

    sort([ ABC, ABC, ABB, ABCDE, A, B, B, B ]) = [ ABCDE, ABC, ABB, B, A ]

<!--more-->

---

**Solution**: Our solution is functional by nature. There are two functions: ```occurrences```, which takes a list of words and return a list of (word, occurrences)
pairs, and ```sort```, which takes the result of ```occurrences```, sort the pairs according to the required order (first by length, then by occurrences) and strips
the occurrences amount from each pair, so the result will only contain the words.

The following is a Scala implementation:

``` Scala
    class Sorter {
    
      def sort(words: List[String]): List[String] = 
        occurrences(words).toList
          .sortBy(r => (-r._1.length, -r._2))
          .map(r => r._1)
      
    
      def occurrences(words: List[String]) = 
        words.groupBy(l => l)
          .map(t => (t._1, t._2.length))
    
    }
```

Have any questions / remarks? leave them at the commonts below!
