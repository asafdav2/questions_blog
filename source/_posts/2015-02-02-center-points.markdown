---
layout: post
title: "center points"
date: 2015-02-02 17:55:02 +0200
comments: true
categories: [Binary Tree, Java]
---

**Question**: We are given a set of points on the $$x$$ axis; Each point is represented by its $$x$$ coordinate.
A point is called **central** if the distance between it to any other point in the set is at most 10.
Design and implement a data structure that supports the following three operations:

* ```insert(x)```, ```delete(x)``` - insertion / deletion of point from the set.
* ```center(x)``` - returns true if $$x$$ exists in the set and is central.

<!--more-->

---

**Solution**: We will store the points in a balanced tree (such as [AVL tree](http://en.wikipedia.org/wiki/AVL_tree), [Red-black tree](http://en.wikipedia.org/wiki/Red%E2%80%93black_tree) or [B-Tree](http://en.wikipedia.org/wiki/B-tree). Specifically Java's [TreeSet](http://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html) is implemented using a Red-Black tree). This will ensure insertion / deletion in $$O(lgn)$$ time complexity.
To find if a point is central we can get the minimum and maximum values from the set ($$O(lgn)$$ as well) and check if the distance between ```x``` to both the min/max values is at most 10.

The following is a Java implementation:

``` Java
    class CenterPoints {
        public static final double CENTER_DISTANCE = 10;
        private TreeSet<Double> tree = new TreeSet<>();
    
        public void insert(double x) {
            tree.add(x);
        }
    
        public void delete(double x) {
            tree.remove(x);
        }
    
        public boolean center(double x) {
            if (!tree.contains(x)) {
                return false;
            }
            final double min = tree.first();
            final double max = tree.last();
            return ((x - min) <= CENTER_DISTANCE) && ((max - x) <= CENTER_DISTANCE);
        }
    }
```
