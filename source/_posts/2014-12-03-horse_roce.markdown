---
layout: post
title: "Car Race"
date: 2014-12-03 21:55:07 +0200
comments: true
categories: [Javascript]
---

**Question**: In this question we model a car race. The race is initialized with a length and an array of speeds for each car. When a car wins, you should
print an appropriate message to the console. During the race, the client can call update(car_id, speed).

**Solution**:

``` Javascript
    function Race(len, speeds) {
        this.speeds = speeds;
        this.remain = [];
        this.lastUpdate = getMillis();
    
        for (var i = 0; i != speeds.length; i++) {
            this.remain.push(len / speeds[i]);
        }
    
        this.triggerTimeoutForLeader();
    }
    
    Race.prototype.triggerTimeoutForLeader = function() {
        var min = Math.min.apply(Math, this.remain);
        var minId = this.remain.indexOf(min);
        if (this.to) {
            clearTimeout(this.to);
        }
        this.to = setTimeout(function(){ winner(minId) }, min * 1000);
    };
    
    Race.prototype.update = function(id, speed) {
        var millis = getMillis();
        var elapsed = (millis - this.lastUpdate) / 1000;
        this.lastUpdate = millis;
        for (var i = 0; i != this.remain.length; i++) {
            this.remain[i] = this.remain[i] - elapsed;
        }
        this.remain[id] = this.remain[id] * this.speeds[id] / speed;
        this.speeds[id] = speed;
        this.triggerTimeoutForLeader();
    };
    
    var winner = function(id) {
        console.log('Car', id, 'has won the race!');
    };
    
    var getMillis = function() {
        var d = new Date();
        return d.getTime();
    }
```
Have any questions / remarks? leave them at the commonts below!
