---
layout: post
title: "rectangles store"
date: 2014-12-18 14:57:53 +0200
comments: true
categories: [Data Structures, Java]
---

**Question**: Your task is to implement a class that stores rectangles. A rectangle is defined by the following interface:

``` Java
    public interface Rectangle {
        int getLeft();
        int getTop();
        int getRight();
        int getBottom();
    }
```

You should implement the following interface:

``` Java
    public interface RectanglesStore {
        void initialize(Rectangle bounds, Collection<Rectangle> rectangles);
        Rectangle findRectangleAt(int x, int y);
    }
```

The arguments to the ```initialize``` method are a rectangle that represents the bounds (e.g. the area in which other rectangles can appear), and a collection of rectangles.
You are required to store these rectangles in an efficient manner (in terms of memory consumption), and to later return the topmost rectangle per specified $$x$$, $$y$$ location (or null in case no rectangle exists in the specified location) in the most time efficient way.

Note: The solution should support a large number of rectangles, and an extremely large bounding rectangle, so the following solutions are invalid:

* a simple collection of rectangles (isn't efficient enough performance-wise)
* a map of each point to its corresponding rectangle (isn't efficient enough memory-wise)
<!--more-->
---

**Solution**:

In addition to the rectangles themselves, our solution will maintain 4 sorted sets: 

* ```lefts``` - holds the *left* values of all rectangles
* ```rights``` - holds the *right* values of all rectangles 
* ```tops``` - holds the *top* values of all rectangles  
* ```bottoms``` - holds the *bottom* values of all rectangles

Each entry in the preceeding sets will also hold the id of the corresponding rectangle.

Now, given $$x,y$$ coordinates as input to ```findRectangleAt```, we can filter all relevant rectangles:

* ```rLefts``` - list of rectangle id for all ```left``` entries having value *lower* than $$x$$.
* ```rRights``` - list of rectangle id for all ```right``` entries having value *greater* than $$x$$.
* ```rTops``` - list of rectangle id for all ```tops``` entries having value *lower* than $$y$$.
* ```rBottoms``` - list of rectangle id for all ```bottoms``` entries having value *greater* than $$y$$.

The point $$(x,y)$$ is contained in every rectangle whose id appears in the 4 afromentioned lists.
Since we want to return the topmost such rectangle, and since the ids in ```rTops``` are sorted according to an ascending $$y$$ value,
we can simply look for the first id in ```rTops``` that alse appear in ```rLefts```, ```rRights``` and ```rBottoms``` and return it.

The following is a Java solution:

``` Java
    public class RectangleStoreImpl implements RectanglesStore {

        private Rectangle bounds;
        private List<Rectangle> rectangles;
        private int numRectangles;
        private TreeSet<Pair<Integer, Integer>> lefts, rights, tops, bottoms;
    
        private class PairComparator implements Comparator<Pair<Integer, Integer>> {
            @Override
            public int compare(Pair<Integer, Integer> p1, Pair<Integer, Integer> p2) {
                return Integer.compare(p1.getFirst(), p2.getFirst());
            }
        }
    
        @Override
        public void initialize(Rectangle bounds, Collection<Rectangle> rectangles) {
            this.bounds = bounds;
            this.rectangles = new ArrayList<>(rectangles);
    
            final PairComparator comp = new PairComparator();
            lefts   = new TreeSet<>(comp);
            tops    = new TreeSet<>(comp);
            rights  = new TreeSet<>(comp);
            bottoms = new TreeSet<>(comp);

            int numRectangles = 0;
    
            for (Rectangle r : rectangles) {
                if (r.getLeft()   < bounds.getLeft()  ||
                    r.getTop()    < bounds.getTop()   ||
                    r.getRight()  > bounds.getRight() ||
                    r.getBottom() > bounds.getBottom())
                    throw new IllegalArgumentException("rectangle" + r + "is out of bounds");
    
                lefts.add(  new Pair<Integer, Integer>(r.getLeft(),   numRectangles));
                tops.add(   new Pair<Integer, Integer>(r.getTop(),    numRectangles));
                rights.add( new Pair<Integer, Integer>(r.getRight(),  numRectangles));
                bottoms.add(new Pair<Integer, Integer>(r.getBottom(), numRectangles));
                numRectangles++;
            }    
        }
    
        @Override
        public Rectangle findRectangleAt(int x, int y) {
            if (x < bounds.getLeft() || x > bounds.getRight() ||
                y < bounds.getTop()  || y > bounds.getBottom()) {
                throw new IllegalArgumentException("requested point is outside of bounding box");
            }
            final Pair<Integer, Integer> px0 = new Pair<Integer, Integer>(x, 0),
                                         py0 = new Pair<Integer, Integer>(y, 0);
    
            final List<Integer>   rLefts = new ArrayList<>(),
                                   rTops = new ArrayList<>(),
                                 rRights = new ArrayList<>(),
                                rBottoms = new ArrayList<>();
    
            for (Pair<Integer, Integer> p :   lefts.headSet(px0, true)) {  rLefts.add(p.getRight());}
            for (Pair<Integer, Integer> p :    tops.headSet(py0, true)) {   rTops.add(p.getRight());}
            for (Pair<Integer, Integer> p :  rights.tailSet(px0, true)) { rRights.add(p.getRight());}
            for (Pair<Integer, Integer> p : bottoms.tailSet(py0, true)) {rBottoms.add(p.getRight());}
    
            // since rTops is ordered from top down, the first rectangle found is the topmost
    
            for (Integer numRect : rTops) {
                if (rBottoms.contains(numRect) &&
                    rLefts.contains(numRect) &&
                    rRights.contains(numRect)) {
                    return rectangles.get(numRect);
                }
            }
    
            return null;  // no rectangle was found
        }
    
    }
```

Have any questions / remarks? leave them at the commonts below!
