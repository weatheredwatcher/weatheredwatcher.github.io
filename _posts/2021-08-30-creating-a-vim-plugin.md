---
title: "Creating a Vim Plugin"
date: 2021-08-30T10:45:42-05:00
categories: [linux]
tags: [linux, vim, dev]
type: "post"
---

Hey everybody!  So years ago I made a plugin for the Mac only text
editor, TextMate, that incorporated the api for Hipster Ipsum to include
some sweet artisinal filler text.  I would like to make a vim plugin
that does the same thing.  I've never made a vim plugin before, so this
is a good lesson on how it is done. The first thing that I am doing is
getting this to work as a function using vimscript in the vim.rc file.

So starting out, I am making a function called ```Hipster()```. It is
important to note that functions in vimscript must start with a capital
letter.

```
function Hipster()

endfunction
```

Next we are going to pull the filler into a variable using the system
command curl.

```
let hipster = system("curl -s 'https://hipsum.co/~&sentences=3'")
```

If you look at the api and run the command on the command line, you see
it puts the filler text into an array.  The returned text is

```
["Wayfarers shoreditch subway tile hot chicken. Etsy green juice gochujang brunch farm-to-table selvage. Activated charcoal fingerstache lomo beard."]
```

So, I really do not need the [] on the ends of the filler, so I am going
to remove them before I drop the filler into the buffer.

To do this, we use the ```substitute()``` function as well as ```:put```
to dump the filler into the buffer.

```
:put =substitute(hipster, '[^a-zA-Z0-9]', '', '')
```

Finally, we map the function to a key:

```
map h :echo Hipster()<cr>
```

All together now!

```
function Hipster()

    let hipster = system("curl -s 'https://hipsum.co/api/?type=hipster-centric&sentences=3'")

    :put =substitute(hipster,'[^a-zA-Z0-9]', '', '')
    endfunction
    map h :echo Hipster()<cr>
```

So now <leader>h will dump 3 lines of artisinal filler into the buffer!
This let's me drop in text in my pages when I do not have anything to
put in yet!

Next up I will take this function and turn it into a plugin that can big
distributed.

