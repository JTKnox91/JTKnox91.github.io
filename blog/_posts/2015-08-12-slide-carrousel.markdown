---
layout: post
title:  "Smoother Slide Carrousel with JQuery"
date:   2015-08-12 01:44:36 -0800
categories: jquery throttle slide
---

## The Setup

I was working on a news feed type display that would live update while the user was viewing the page. The animation in pseudo-code was:

1.  Add new element top top of list, with display: none
2.  Slide down first element (the new one)
3.  Slide up last element (the oldest one)
4.  Delete last element from the DOM.

Like this:

![img](http://jtknox91.github.io/images/Screen-Shot-2015-07-26-at-11-17-56-PM.png)

Even though 2 and 3 are separate statements, ideally I want both to happen at the same time, for the smoothest visual effect.

I also want the slide up animation for the oldest element to run for its full duration before removing that element from the DOM, to keep things as smooth as possible.

##### Original Code: 

    var updateAnimation = function(newestPost) {
      newestPost
        .hide()
        .prependTo($("#news-feed"))
        .slideDown(600);
      $(#news-feed:last-child).slideUp(600);
      $(#news-feed:last-child).remove();
    };


###### Fortunately...

The JQuery .slideDown() and .slideUp() animations already run near simultaneously. That's one issue down.

## The Problem

###### However...

The .slideUp() animation and .remove() also run near simultanesouly. What I got was a few mili-seconds of the oldest post shrinking up before blinking out of site. This looked "jumpy" and I wanted "smooth".

## The solution

The makers of JQuery thought this might be an issue too. Let's take a look at the [full documentation](http://api.jquery.com/slidedown/) for a slide animation. (I'll use slideDown for example).

![img](http://jtknox91.github.io/images/Screen-Shot-2015-07-26-at-10-33-04-PM.png)

.slideDown() actually takes a second argument, "complete", which should be a function. The "complete" function will not run until *after* the sliding animation is complete. How convenient!

#### New Code:

    var updateAnimation = function(newestPost) {
      newestPost
        .hide()
        .prependTo($("#news-feed"))
        .slideDown(600);
      $(#news-feed:last-child).slideUp(600, function() {
        $(#news-feed:last-child).remove();
      });
    };

## A new problem Occurs.

#### What if I call update updateAnimation too frequently?

The updateAnimation function removes the last-child of the news feed, but not until 600ms after the function is called.

*Suppose I called updateAnimation, then called it again 500ms (half a
second) later:*

* 0ms (First call):
  * *Call 1*: Preprend new element prepended
  * *Call 1*: First new element slide down
  * *Call 1*: Last element slide up
  * (Last element is hidden, but still in the DOM)
* 500ms (Second call):
  * *Call 2*: Second new element prepended
  * *Call 2*: Second new element slide down
  * *Call 2*: Last element (the hidden one) slide up (and stay hidden)
  * (There is only one hidden element, and nothing has been removed).
* (600ms)
  * *Call 1*: Finish both sliding animations
  * *Call 1*: Last element (the same hidden one) remove
* (1100ms)
  * *Call 2*: Finish slide down animation
  * (The element that slid up from call 2 has already been removed)
  * *Call 2*: Last element (a different, unhidden one) remove

In short, this will slide down two new elements while only sliding up one, and then "blink" out a second element a half second later, creating an aesthetic that feels sporadic, rather than smooth.

#### Never Fear. Underscore is here!

###### \_.throttle()

If you don't already use [underscore.js](http://underscorejs.org/), I would strongly advise it. `_.throttle(function, wait, [options])` wraps `function` so that it will not be called unless `wait` seconds have passed since its last call. If a throttled function is called during `wait` then `function` will be queued to run after `wait` has passed.

**Note:** \_.throttle will only queue up the next call. If multiple calls are made during the wait period, only one call will be made after the wait. In my case, I was only concerned with smooth animations, not displaying 100% of content.

#### Final Code:

    var animateSpeed = 600; //ms
    var updateAnimation = _.throttle(function(newestPost) {
      newestPost
        .hide()
        .prependTo($("#news-feed"))
        .slideDown(animateSpeed);
      $(#news-feed:last-child).slideUp(animateSpeed, function() {
        $(#news-feed:last-child).remove();
      });
    }, animateSpeed + 50);

There you have it. My page has been struck by a "Smooth Carrousel."

#### Good luck, have fun.
