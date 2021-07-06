---
date: "2009-08-22T00:00:00Z"
date_published: "2009-08-22T04:00:00.000Z"
date_updated: "2009-08-22T04:00:00.000Z"
slug: adventures-in-ruby
title: Adventures in Ruby
---

\n    I threw up a quick site from iWeb for my daughter not too long ago and I've decided to create a custom site for her all in Ruby on Rails.  This is mainly to test out rails a bit more so I can form a decent opinion of it vs. php.  Today, I also decided to see how Textmate  does as a Rails coder vs. Coda (which I use mostly now).  I recently ran an update of rails on the mac and when I started to configure the site by typing rails foto_devochka, all I got was a string of errors.  I tried upgrading everything...no luck.  I finally found a post on a Rails site that seemed to address my issue.  They recommended:

    sudo gem update
    sudo gem update --system
    sudo gem install rails
    

After this completed, rails worked again.  I created the app and tried to view it in the browser.  No dice.  I checked the dbase configuration.  Looked good.  Checked the server output.  It was complaining about sqlite.  Sqlite ships with Mac, so I've never installed it before, but apparently, when I reinstalled rails, it lost all the gems. So:

    sudo gem install sqlite3-ruby
    

Did the trick.

With that little technical garbage out of the way, here is my setup for Textmate.  I'm using Textmate in one space for development, a terminal in another space for server/console and I have safari loaded in a third space for testing.  I can easily switch between them using key-strokes, or clicking on the right icon in the dock.  The only issue that I can see is the one of ftp'ing stuff.  In Coda it is automatic.  I'll need yet another program to upload files with this set-up.

I admit, I still use Mac's terminal with Coda.  I also use Safari for previewing.  So I'm not really using Coda to it's "full" potential here.  I do use Coda to take a peek at my views, and I have used it for terminal stuff, but it is really just as easy to use the bash shell that comes natively with Mac.  It's that damn ftp feature that I love so much.  I seriously doubt that I'll ever use Textmate for full blown Rails or php development.  It'll make a great text editor though.  It really is the culmination of emacs.

Let's get back to the site, shall we.  This is not my first site in Rails, so I quickly setup a framework and modify the routes.rb file.  Soon, my pages are working, just need a template now for the main pages.

Here are the pages that I'll need:

    script/generate controller Site index about gallery about
    

Then I create a site layout file.  IT contains the links.  A great little timesaver in rails is the

    link_to_unless_current
    

This creates the link, but will not link to it if is the current page.  Makes for a bit cleaner page.  I don't use it everywhere, but it's a good thought.

Now I just need some ajax goodies, so I'll load a bunch of javascript libraries that I'll need:

    <%= javascript_include_tag "prototype.js" %>
    <%= javascript_include_tag "scriptaculous.js?load=effects,builder" %>
    <%= javascript_include_tag "lightbox.js" %>
    

Wow, it's looking good and in no time!   I do like the flow but now that I've got a little something to upload, I know I'll start missing Coda.  Okay, so I'm also going to miss Coda when it comes to css.  Coda has spoiled me with it's css editor.

Well, that is it for now.  I'll be finishing up this site (in Coda) and then posting it to the web.  I'll post a link when it's fully functioning
