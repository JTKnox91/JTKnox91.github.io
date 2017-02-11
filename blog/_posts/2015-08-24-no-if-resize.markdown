---
layout: post
title:  "No ifs, ands, or..."
date:   2015-08-24 00:00:00 -0800
categories: stupid-ideas hashtable
---

[This](https://en.wikipedia.org/wiki/Hash_table) is a Hashtable.

I won't go into specifics about it in this post, but its an awesome data structure that allows you to assign and lookup in constant time. Those familiar with hash tables know that they occasionally require resizing. In javascript, it would look something like this:

    if (limit / items >= .75) {
         return limit * 2;
    }
    if (limit / items < .25) {
        return limit / 2;
    }

Simple, enough, right?

Well a few days ago, someone proposed writing the entire re-sizing
function **without an 'if' statement**... ...and I took the bait.

#### The final solution

If you're not particularly patient, this is where I (with some great help of my room mates) arrived at:

    function newLimit(items, oldLimit) {
      return oldLimit * (
          Math.pow(2, Math.floor(
            (items/oldLimit)*(4/3)
          ))/
          Math.pow(2, Math.ceil(
            ( (oldLimit - items) / oldLimit) * (4/3) -1
          ))
      );
    }

#### How does it work?

Well, another way to think about the re-sizing function is that it
returns one of the following:

* `Limit * 2 (when above 75% capacity)`
* `Limit * 1 (when no change is needed)`
* `Limit * 1/2 (when below 25% capacity)`

Another way to think of this is:

* `Limit * 2^1/2^0`
* `Limit * 2^0/2^0`
* `Limit * 2^0/2^1^`

Or as one line:

* `Limit * 2 ^ someFunctionToMake1or0(limit, items) / 2 ^ someFuctionToMake0or1(limit,items)`

###### someFunctionToMake1or0 (Numerator) {#somefunctiontomake1or0numerator}

* (items/limit) will produce a number between 0 and 1
* multiplying (items/limit) by 4/3 will result in a number greater
    than 1 only if (items/limit) &gt; 3/4
* calling Math.floor on the that entire sequence will result in either
    a 0 or a 1 as we need

**full implementation:**

    function numeratorFunc(items, limit) {
        return 2 * Math.floor((items/limit) * 4/3);
    }

###### someFunctionToMake0or1 (Denominator) {#somefunctiontomake0or1denominator}

This one was a bit trickier.

* we're gonna use the difference of the of items and limit instead of
    the number of items
* ((items-limit) / limit) will cross the threshold of .75 when its
    time to shrink the hashtable
* and just like like before we can multiply that result times 4/3,
    moving our threshold to 1
* just like before when can get the floor of that to make 0 or 1

**almost done:**

    function denominatorFunc(items, limit) {
        return 2 * Math.floor((limit-items)/limit) * 4/3);
    }

* you double in size when your capacity is &gt;= 75%
* but you half in size only when your capacity &lt; 25%
* this is where things get stupid...er

**full implementation:**

    function denominatorFunc(items, limit) {
        return 2 * Math.ceil((limit-items)/limit) * 4/3) -1;
    }

* Yes. We called Math.ceil()... and then subtracted 1. But hey it
    works!

#### Now back it up

We can combine both to get:

    function newLimit (items, oldLimit) {
        return numeratorFunc(items, oldLimit) / 
               denominatorFunc(items, oldLimit);
    }

And by writing out both of those inner function (with some attempted
lexical spacing) we get the solution at the start of this post:

    function newLimit(items, oldLimit) {
      return oldLimit * (
          Math.pow(2, Math.floor(
            (items/oldLimit)*(4/3)
          ))/
          Math.pow(2, Math.ceil(
            ( (oldLimit - items) / oldLimit) * (4/3) -1
          ))
      );
    }

And **if you just hate things that make sense**, here's a one liner:

    function newLimit(items,oldLimit){return oldLimit*(Math.pow(2,Math.floor((items/oldLimit)*(4/3)))/Math.pow(2,Math.ceil(((oldLimit-items)/oldLimit)*(4/3)-1)));}

You're welcome.

###### Notes

* There's a [github repo](https://github.com/JTKnox91/no-if-resize) for this.
* I contemplated using boolean typecasting "+(items/limit &lt; .25)" to convert logical expressions to 1s or 0s, but decided it was against the spirit of the problem, plus that kind of behavior is not as common in all languages as floor and ceiling functions.

#### Good Luck, have fun