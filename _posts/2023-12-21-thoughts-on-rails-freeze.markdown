---
date: "2023-12-21"
date_published: "2023-12-21"
date_updated: "2023-12-21"
slug: "thoughts-on-rails-freeze"
title: Thoughts on Rails Freeze
tags: [dev, rails]
type: "post"
---

So I saw a post about a Rails function ```freeze``` that caught my attention.  I would like to speak on it a bit.  So basically, for the uninitiated,
The freeze method lets you set an object in rails to be immuttable.

For example:

```ruby
name = "Robert"
new_name = name

name[0] = T

name       #Tobert
new_name   #Tobert
```

See, in this instance, we have created a link between the object, ```name``` and ```new_name```.  Change one and we change the other one.

This is where ```freeze``` comes in.  If we freeze name: ```name.freeze``` then try to change name directly, we get an error.  If we change
```new_name``` it will not change ```name```.

So if right be the change I add the code:

```ruby
name.freeze
```

I'll get an error message ```can't modify frozen String: "Robert" (FrozenError)```
