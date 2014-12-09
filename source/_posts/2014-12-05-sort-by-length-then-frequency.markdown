---
layout: post
title: "Sort by length then frequency"
date: 2014-12-03 19:32:20 +0200
comments: true
categories: [Scala]
---

**Question**: Implement a function that takes a file name of text file that contains words (each in a different line, 
words may apper more than once), and returns the list of words (without duplications), sorted first by word 
length (descending) and then by number of occurrences (descending).

In other words, the longest word will appear first in the list. In case there are several words with the same
length, their inner order will be according to their appearences frequency.

**Example**: assume the file ```words.txt``` contain the following content:

    ABC
    ABC
    ABB
    ABCDE
    A
    B
    B
    B

Then running ```sort("words.txt")``` should return the following result:
 
    ABCDE ABC ABB B A

---

**Solution**:

The following is a Scala implementation:

``` Scala
    class Sorter {
    
      def sort(filename: String): List[String] = sort(scala.io.Source.fromFile(filename).getLines().toList)
    
      def sort(words: List[String]): List[String] = 
        occurrences(words).toList
          .sortBy(r => (-r._1.length, -r._2))
          .map(r => r._1)
      
    
      def occurrences(words: List[String]) = 
        words.groupBy(l => l)
          .map(t => (t._1, t._2.length))
    
    }
```
