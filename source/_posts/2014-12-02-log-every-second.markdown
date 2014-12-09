---
layout: post
title: "log every second"
date: 2014-12-02 21:54:15 +0200
comments: true
categories: [Javascript]
---

**Question**: Please write a program which will log an increasing integer, once every second.

---

**Solution**:
The following is a simple Javascript implementation:

``` Javascript
    var i = 0;
    var delay = 1000;
    var logger = function() {
      ++i;
      console.log(i);
      setTimeout(logger, delay);
    }
    
    logger();
```

**Follow-up Question**: Can we be centain that the delay between two consecutive loggings will be exactly 1 second? If not, how would you improve it?

**Solution**: An important concept to remember is that with Javascript timers, delay is not guaranteed. The browser operates in a single thread manner, processing
events that accumulate in an events queue and handles them sequentially. This means that there may be a slight delay between the moment that the event was fired and
the moment that the browser started to process it.

To improve the delays accurancy we can run this code in a dedicated thread, by utilizing a [web worker](https://developer.mozilla.org/en-US/docs/Web/Guide/Performance/Using_web_workers).


