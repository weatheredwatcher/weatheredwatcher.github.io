---
date: "2024-02-12"
date_published: "2024-02-12"
date_updated: "2024-02-12"
slug: "using-caddy-to-develop-ror"
title: Using Caddy to Develop RoR
tags: [dev, ruby, rails, caddy, devops]
type: "post"
---

Working on a few projects in rails and I wanted to do something where I could load the site in the browser with a tld and an ssl certificate
like people.  So going to https://conflict.dev would take me to my local runing dev server for my conflict app.  And if I had other things I
was working on at the same time, I wanted them to also be able to be loaded in the same way.  Oh, and I need them to have ssl certificates so
I can test with that as well!

I read a few posts and came to the conclusion that Caddy and a reverse proxy are what I want.  In short, I run Caddy and have it reverse proxy
my site on port 3000 and publish it to 80 and 448.

Here are the steps that I took to accomplish this:

First thing I want to do is create some signed certificartes in my site root.

```mkcert domain.tld```


Then I need to make sure I am loading puma and my rails apps on distinct ports.  I created a rake task called local for this

```ruby
desc "launching the rails server on a set port"
task :local do
sh  "rails s -p 3040 --daemon"
sh  "caddy start"
end
```

As you can see, I am starting puma on port 3040 here and also starting Caddy.

For that last line to work I need to create a file in my project root Caddyfile

```[yaml](yaml)
domain.tld
reverse_proxy localhost:3040
tls domain.tld.pem domain.tld-key.pem
```

This sets up the domain and the ssl certificates for the site.

In order to make sure that rails security recognizes your new domain you will need to edit ```config/environments/development.rb```
Just add the line:

```ruby
config.hosts << "domain.tld"
```

The last thing that we need to do is setup the hosts records.
It's as easy as adding ```127.0.0.1 domain.tld```

Now, to get this all up and running, I just run ```rake local``` and all the services are spun up and I can goto my site in the browser
as if it were a live site!
