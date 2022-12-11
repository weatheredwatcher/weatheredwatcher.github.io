---
date: "2013-01-24T00:00:00Z"
date_published: "2013-01-24T05:00:00.000Z"
date_updated: "2013-01-24T05:00:00.000Z"
slug: running-multiple-apps-on-nodejs
title: Running Multiple Apps On NodeJs
---

So, what I am wanting to so is to be able to run multiple apps on Nodejs.  Specifically, I want to be able to use node-static to server static files and some other app (yet to be determined) to server up my blog as flat files.  I've done this before in Ruby using Rack and Sinatra...so I figure I would give Nodejs's Bogart a try!\n 

With a little bit of trial and error, I have come up with the best solution to this: Http-Proxy.

The first thing to look at will be my packages.json file.  

    {
        "name": "bogart-test",
        "description": "Testing Bogart/FlatFile/Static structures",
        "version": "0.1.0",
        "author": "David Duggins",
        "email": "David Duggins",
        "main": "./app",
        "directories": { "lib": "./lib" },
        "dependencies": {
          "node-static": ">=0.6.5",
          "bogart": ">=0.2.0",
          "mustache": "0.3.1-dev",
          "http-proxy": ">=0.0.0"
        }
    }
    

The important stuff to note is node-static, bogart and node-static. I have not started to use mustache yet, but that may or may not be the templating engine.

Bogart by itself is fairly straight-forward.  It's just as easy to configure as Sinatra is for Ruby or Silex for php.  It's just handles routes.

    var bogart = require('bogart');  
    var router = bogart.router();
    
    router.get('/', function(req) {
    
      return bogart.html("hello world");
    });
    
    var app = bogart.app();  
    app.use(bogart.batteries); // A batteries included JSGI stack including streaming request body parsing, session, flash, and much more.  
    app.use(router); // Our router
    
    app.start();  
    

The above example with simply echo "Hello World" on the index of our site.  It is set to use the default port 8080.  That cam be easily changed with     app.start('10000', '127.0.0.1')

The next part is node-static.  I want to be able to serve static files, like an about page.  Fairly simple as well:

    var static = require('node-static');
    
    //
    // Create a node-static server to serve the current directory
    //
    var file = new(static.Server)('.', { cache: 7200, headers: {'X-Hello':'World!'} });
    
    require('http').createServer(function (request, response) {  
        request.addListener('end', function () {
            //
            // Serve files!
            //
            file.serve(request, response, function (err, res) {
                if (err) { // An error as occured
                    console.error("> Error serving " + request.url + " - " + err.message);
                    response.writeHead(err.status, err.headers);
                    response.end();
                } else { // The file was served successfully
                    console.log("> " + request.url + " - " + res.message);
                }
            });
        });
    }).listen(1337);
    

This code merely pulls any static files and servers them. It requires that you use naming conventions like index.html to make sure that a file is pulled up via '/'.  You also can call other pages just like you would on a normal apache server. 

The final part of this is configuring Bogart to use Http-proxy so that we can load the static pages only when we want to. 

 To load http-proxy we need these two lines:

    var http = require('http')  
    , httpProxy = require('http-proxy');
    

Then to use a proxy, we need this line: 

      router.get('/', function(req) {
      return bogart.proxy('http://127.0.0.1:1337');
    });
    

Remember that the static app is running on port 1337. 

 Well that is all for now.  I will be working on the other parts of this experiment and write more on it later.
