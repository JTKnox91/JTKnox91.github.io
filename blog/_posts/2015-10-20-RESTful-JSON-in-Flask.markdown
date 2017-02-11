---
layout: post
title:  "RESTful JSON in Flask"
date:   2015-10-20 00:00:00 -0800
categories: REST Flask
---

## Sending and recieving JSON with flask
So you followed a flask tutorial and now you have a pretty html page that says hello world. Maybe it says more than just that, but you would still like to escalate past serving static html files. **No problem!** This article will show you how to build a simple RESTful API.

#### Requirements

```python
from flask import Flask, request, jsonify,
```

###### Explanation
* `Flask` is exactly what you'd expect
* `request` will allow you to grab the method, headers, and content from a request
* `jsonify` will be used to make `response` objects

#### Request Handlers

    @app.route('/api/items/<item_id>',methods=['GET', 'POST'])
    def items_handler(item_id):
        if request.method == 'GET':
            #do things with item id and send back a response...
        if request.method == 'POST':
            body = request.get_json() # 'body' is now a python dictionary or list
            # do things with body and send back a response...

###### Explanation

* The `@app.route` function decorator will send any requests to `/api/items` through this function
* `<item_id>` Allows us to pass additional information through the url. Its optional, but the name in brackets from the URL should match the name `items_handler` arguments
* Anything other than a `GET` or `POST` request will be automatically responded to with an error.
* As for making responses...

#### Responding

    if request.method == 'GET':
        if not some_function_that_verifies_item_exists(item_id):
            return "Could not find item", 404
        data = some_function_that_gets_info_from_the_db()
        response = jsonify(data)
        return response, 200


    if request.method == 'POST':
        body = request.get_json() # 'body' is now a python dictionary or list
        new_item = some_function_that_adds_to_db(body['item'])
        response = jsonify(new_item)
        return response, 201

###### Explanation

* In the bottom example, its assumed the body of our request has a property called 'item'.
* `jsonify()` is making a response object, and we conveniently naming that object `response`
* Flask allows for custom status codes, without specificing something like `return response, 201`, all non-error responses will be 200

Now you know how to make a RESTful API with flask

#### Good luck, have fun