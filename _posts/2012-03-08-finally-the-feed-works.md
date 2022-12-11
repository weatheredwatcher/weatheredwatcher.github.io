---
date: "2012-03-08T00:00:00Z"
date_published: "2012-03-08T05:00:00.000Z"
date_updated: "2012-03-08T05:00:00.000Z"
slug: finally-the-feed-works
title: Finally, the feed works
---

I finally had the time to sit down and write a RSS feed for my blog.  I obviously wrote it in php using codeigniter.  There are a few good tutorials out there, but let me add my own ten cents to the pile!!\n
 So the first spot is the controller.  I created a controller called Feed.  I also could have just created a method under my Blog controller, but I find this is a little bit neater code.

    function index()
    {
        $this->load->helper('inflector');
            $this->load->helper('xml');  
        $this->load->helper('text');  
        $this->load->model('Blog_model', 'blog');    
    
        $data['entries'] = $this->blog->get_last_ten_entries();
        $this->load->view('rss', $data);
    
    
    }
    

This loads the required CI helpers, as well as by Blog Model.  Notice that I am using a custom method in my model to retrieve the last ten entries.  I am reusing the same method that populated my main blog page.

The first thing to remember regarding the xml page that you define in your view is opening the xml using the echo statement.  The syntax would otherwise confuse the compiler.  After that we just use pure XML and insert our dynamic data with php short tags.  Surprisingly enough, that is it!
