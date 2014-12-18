---
layout: post
title: "Celebrities"
date: 2014-12-15 19:32:20 +0200
comments: true
categories: [Algorithms, Scala]
---

**Question**: Let us define a *celebritiy* as a person who is known by everyone, yet knows no one.
Assume you have a function $$knows(A,B)$$ which returns TRUE if $$A$$ knows $$B$$ (and runs in constant time).
Implement a function that finds the celebrities in a given list of people. This function should run in linear time complexity.

**Example**: If the people list is $$[A, B, C, D]$$ and the following relations apply:

    A knows B
    A knows D
    C knows B
    C knows A
    D knows B

Then the result should be $$B$$.
 
---

**Solution**: First, we can easily deduce that according to the definition, there can only be one celebrity
in a group of people (if both $$A$$ and $$B$$ are celebrities, then $$A$$ must know $$B$$ and $$B$$ must know $$A$$, but a celebrity knows no one).

The main idea is that given 2 potential celebrities, $$A$$ and $$B$$, we can ask ```knows(A,B)``` and eliminate one of them (if $$A$$ knows $$B$$ then we can eliminate $$A$$, else we can eliminate $$B$$). 
The actual algorithm is as follows: build a potential celebrities pool - a linked list containing all people. Iterate over the list in pairs, and for each pair eliminate one people as described above, and remove him from the list.
When we reach the end of the list, we've eliminated half of the population. Apply the elimination step iteratively until the list length is one. This is the celebrity.

The time complexity for each iteration is linear in the size of the population. Since we eliminate half of the population in each step, the total running time is linear as well ($$n + \frac{n}{2} + \frac{n}{4} + \frac{n}{8} + \ldots \larrow 2n$$).

The following is a Scala implementation:

``` Scala
    class Celebrities {
    
        def diminish(people: List[Int]): List[Int] = people match {
            case Nil => Nil
            case a :: b :: rest => (if (knows(a,b)) b else a) :: diminish(rest)
        }
        
        def celebrity(people: List[Int]): Int = 
            if (people.length == 1) people.head 
            else celebrity(diminish(people))
    
    }
```
