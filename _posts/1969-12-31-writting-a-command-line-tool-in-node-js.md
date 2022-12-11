---
date: "1969-12-31T00:00:00Z"
date_published: "1970-01-01T00:00:00.000Z"
date_updated: "2019-06-05T14:12:47.063Z"
draft: true
slug: writting-a-command-line-tool-in-node-js
title: Writting a Command Line Tool in Node Js.
---

As I have been doing more and more in Node JS, it hit me that I have made a few sites, but never a command line tool.  I am a big fan of the command line.  So I thought, let me make a neat little tool just to see how it all fits together.

To see the finished product [check this out](https://github.com/weatheredwatcher/name-gen).

Ok, so lets get this party started!

Two main items of importance are the package.json file and the bin file.

    {
        "name": "name-gen",
        "version": "1.0.0",
        "license": "MIT",
        "bin": {
            "name-gen": "bin/name-gen"
        },
        "scripts": {},
        "devDependencies": {
            "chai": "^4.2.0"
        },
        "dependencies": {
            "minimist": "^1.2.0"
        }
    }
    

Aside from the dependencies (minimist for the app and chai for testings) notice the "bin" node.  I am pointing to a file in a folder called bin/. This file has details on running the application.

The file `bin/name-gen` contains a single line `require ('../src/')()`.  It's loading all the code in my source folder.

In the src/ folder you will find a Node JS application.
