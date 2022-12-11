---
title: "Using Docker with PHP Composer"
date: 2021-10-17T10:45:42-05:00
categories: [linux]
tags: [linux, docker, dev, php, composer]
type: "post"
---

So in this installment, I will be talking about how to use PHP/Composer
with Docker.  I am currently having to work with a Mac for a contract. I
could use homebrew and get PHP setup on the Mac, but I am having to deal
with multiple versions of PHP and frankly, I really do not want to have
to deal with PHP on a machine that is not really mine.  I really do not
even have to run php locally, I am using a dev docker container to serve
up the site locally. So I really just need to be able to run Docker.
It's not a very difficult proprosal.

First thing we must do is prepare a few things in the bash or zsh rc
file.  I use ZSH, so this is in my .zshrc file, but it works also with
.bashrc or .profile.  Whatever you use!

So the first step is to make a function in our bashrc file:

    function docker_this_directory() {
        docker run -it --network host --rm -v "$(pwd)":$1 -w $1 $(@:2)
    }

Then we need to create the alias for composer.

    alias composer="docker_this_directory /directory composer:latest"
