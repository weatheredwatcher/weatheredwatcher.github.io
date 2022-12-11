---
date: "2009-12-17T00:00:00Z"
date_published: "2009-12-17T05:00:00.000Z"
date_updated: "2009-12-17T05:00:00.000Z"
slug: syncing-with-google-calendar-all-of-them
title: Syncing with Google Calendar (all of them)
---

\n    I have been using Google "products" for years now.  When every something new comes out, I just have to give it a try.  I love Google Notebook, but it seems that not enough people shared my love.  Of course Gmail can be called life changing.  Another great product that has gotten me through a lot is Calendar.  Being able to sync work and classes and family events in a place that is accessible anywhere is a very cool thing indeed.    I also have to say that I love my Mac and my iPhone.  Syncing the three is fairly easy.  You subscribe to your calendars in iCal, and sync iCal with the iPhone.  The only thing is, my iPhone is always with me, the Mac and iCal not always.  So it became necessary to find a way to create a two way sync between Google Calendar and my iPhone.  Oh yeah, and I have more than one calendar and I want them all to sync.  Using the Exchange server on the iPhone to connect was easy, but I didn't like have the mail was handled.  IMAP works just fine thank you very much.  Also, it only syncs your primary calendar.  If I created other calendars, or subscribe to others, they don't even show up.  I found some articles on how to sync using CalDav and it seemed to be just right for my needs.  To sync your primary calendar, add a new CalDav to the iPHone and enter this in as your url:  [https://www.google.com/calendar/dav/you@gmail.com/user/](https://www.google.com/calendar/dav/you@gmail.com/user)  Add your google username and password and you should be good to go.  Now, for the other calendars, you need to goto your calendars in google and take a look at the ical link.  In the string, you should see a gianormous string @group.calendar.google.com.  Write that down.  Then, add another caldav calendar and in the url enter this: [https://www.google.com/calendar/dav/[string](https://www.google.com/calendar/dav/[string) of numbers and letters]@group.calendar.google.com/user/  Fill in the username and password same as above.  And there you go!  A few things to note:

*You must use https
*The url will disapear in both instances and be repalced my [https://www.google.com](https://www.google.com) It's suppose to

On another note, I have been playing around a bit with Thunderbird 3 and I am impressed with it as a mail client.  I found a Lifehacker article that talked about hacking a tab to display Google Wave permanently and that's made Thunderbird very useful.  I used Thunderbird for my email in Linux so I am comfortable with it.  The only thing that is a bit cumbersome is the calendar extension, Lightning.  Sure I installed a Nightly Build for 3 and it works fine, but Google Calendar Syncing is a bit of an issue.  Once again, I can subscribe as caldav to Google Calendar (use the aboce strings, but change /user to /events) The only issue is that I can't add or change secondary calendars in Lightning.  It has some permissions issues with Google that havn't been worked out yet.  So I applied the same hack that let me have Google Wave in my Thunderbird to give me Google Calendar in my Thunderbird.  The result is a great , quick fix to the Calendar syncing issue!