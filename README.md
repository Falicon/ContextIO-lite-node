Context.IO - Mailboxes know a lot. Use them.
============================================

Context.IO is the missing email API that makes it easy and fast to integrate your user's email data in your application. ContextIO-node-lite is a Node.js client library for the lite API.

Usage of this library requires you to register for a Context.IO API key. You can get one free here: http://context.io/

This code was origionally forked from the official ContextIO-node project that supports the 2.0 version of the API and is located here: https://github.com/contextio/ContextIO-node

Installation
------------

ContextIO-lite-node is installed using npm (http://npmjs.org/)

``` bash
  $ npm install contextio-lite
```

Getting started
---------------

Once you install the contextio package, using it in your code is fairly simple:

``` js
  var ContextIO = require('contextio-lite');
  var ctxioClient = new ContextIO.Client({
    key: "YOUR CONTEXT.IO CONSUMER KEY",
    secret: "YOUR CONTEXT.IO CONSUMER SECRET"
  });
```
 
 The `Client` constructor simply requires your OAuth consumer key and secret. You can also specify the version and endpoint. By default, the client will use the latest stable version of the API (currently 2.0) and http://api.context.io respectively.
 
 Instantiating the client while specifying the API version:
 
``` js
  var ContextIO = require('contextio-lite');
  var ctxioClient = new ContextIO.Client('lite', {
    key: "YOUR CONTEXT.IO CONSUMER KEY",
    secret: "YOUR CONTEXT.IO CONSUMER SECRET"
  });
```

Instantiating the client while specifying the API version and endpoint:

``` js
  var ContextIO = require('contextio-lite');
  var ctxioClient = new ContextIO.Client('lite', 'https://api.context.io', {
    key: "YOUR CONTEXT.IO CONSUMER KEY",
    secret: "YOUR CONTEXT.IO CONSUMER SECRET"
  });
```

Doing calls to the Context.IO API
---------------------------------

Complete documentation is available on http://context.io/docs/latest and you can also play around with the API using the Context.IO Explorer (https://console.context.io/#explore, developer account required).

The design of this library follows the URI structure very closely. For example, to call:

``` http
GET /2.0/users?limit=15
```

you would do:

``` js
ctxioClient.users().get({limit:15}, function (err, response) {
	if (err) throw err;
	console.log(response.body);
});
```

Making it more general, the equivalent of this generic URI:

``` http
METHOD /2.0/RESOURCE/INSTANCE_ID/SUB_RESOURCE?PARAMS
```

would be:

``` js
ctxioClient.RESOURCE(INSTANCE_ID).SUB_RESOURCE().METHOD(PARAMS, CALLBACK_FN)
```

Note that if the resource name contains an underscore character (eg. connect_tokens), you can use both connect_tokens() or connectTokens() with this library.

Note that attachements and raw messages will be sent as a buffer, so you should handle them accordingly. For example, dump the attachment content to the console:

```
  req.contextio_client.users('A_USER_ID').emailAccounts('AN_EMAIL_ADDRESS').folders('A_FOLDER').messages('A_MESSAGE_ID').attachments('AN_ATTACHMENT_ID').get(function(err, response){
    console.log(response.body.toString('utf-8'));
  });
```


### Parameters
Call parameters are passed as an `Object` with properties matching parameter name. Parameters for POST or GET work the same: an Object passed as the first argument of the method call.

### Callback function
Your callback function gets 3 arguments:

  1. **err** Either null or an `Error` if something went wrong
  2. **response** An `Object` representing the HTTP response. It has three properties:
    * *body*: `Object`, `Array` or `String` - If Content-Type is `application/json`, the response body is parsed automatically
    * *statusCode*: `Number` - The HTTP status code of the response
    * *headers*: `Object` - HTTP headers of the response
  3. **request** An `Object` mainly useful for debugging purposes
    * *host*: `String` - Host part of the URL being called
    * *path*: `String` - Path portion of the URL being called
    * *method*: `String` - HTTP method of the call
    * *headers*: `Object` - HTTP headers of the request

Node.js version
---------------

It has been tested on Node.js 0.6.

Examples
--------

Please refer to the test files for an example of every single call.
