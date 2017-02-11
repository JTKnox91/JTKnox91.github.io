---
layout: post
title:  "How to use express sessions"
date:   2015-09-25 00:00:00 -0800
categories: authentication cookies sessions
---

#### What is it for?
Express sessions abstracts the proccess of making cookies for your client and reading data from those cookies.

#### Adding Express-sessions to your server.js file
Add express sessions to your server.js file with:

    var express = require('express');
    var session = require('express-session');

    app.use(session({
      secret: 'somerandomystring',
      resave: false,
      saveUninitialized: true
    }));

For now, don't worry about the properties in that object. For more info, read the [docs](https://github.com/expressjs/session). Express-sessions will handle the act of reading and writing cookie data for you. Just know that **app.use(session(...)), has added a property called "session" to ALL request objects.** Its that simple. 

#### Using express sessions

###### Making a session

After you verify your user's password, you will want to give them a cookie, so they do not have to keep logging in.

    var createSession = function(req, res, userID, next) {
        req.session.regenerate();
        req.session.user = userID;    
        next();
    }

What's goin on here?
* Sessions is an property on the request object. It is also itself an object.
* We can add any property we want to the sessions object. I just chose to add a property called user and set it equal to a userID number that i got from querying my database.
* Finally we call next to let node run its next peice of middlewear.

###### Getting data from a session

Suppose we want to check if a client is logged in before serving them a page, we could make a helper function for that too.
    
    var isLoggedIn = function(req) {
      return !!req.session.user
    };

There are more elegant aproaches for this, but in the above code we are just checking that the session object has a property called user.

#### Good luck, have fun