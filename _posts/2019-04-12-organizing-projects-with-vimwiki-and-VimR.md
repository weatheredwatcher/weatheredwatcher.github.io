---
title:  "Organizing projects and notes with vimwiki and VimR"
date:   2019-04-12
categories: [development]
tags: [development]
---

After a long time of absence I decided to reactivate my blog! So here comes
another post related to optimizing the workflow of a PhD student in Computer
Science.


## The problem

If there is one thing you should do a lot in your PhD studies it is reading.
Papers, course materials, books, blog articles. Over time it becomes hard to
keep track of everything read and the thoughts one had while reading. While it
is possible to track this kind of information (especially for papers) in
reference management software, it is usually not desired/possible to store
information from heterogeneous sources in such a format.


## A potential solution

As I have developed a strong resistance against using practically any other
editor besides (Neo)vim [^1], I searched for a solution within this framework. The
most lucrative solution for my taste was [vimwiki](https://github.com/vimwiki/vimwiki).
It allows to:

 - Organize notes from multiple projects in a hierarchy
 - Supports navigating between these in a flawless fashion
 - It is very easy to create new pages
 - Can be configured to use a markdown compatible syntax
 - As everything is text, changes can also easily be synchronized git and even
   be hosted in a personal wiki on github


## Configuration

I configured vimwiki such that it saves all my notes in a folder `PhDWiki` in
my home directory, uses the markdown syntax and only gets activated when files
inside this notes folder are being edited (I want to have the possibility to
configure my general markdown integration independently of vimwiki). This can
be achieved by adding the following settings to your `.vimrc` or `init.vim`:

```vimscript
let g:vimwiki_list = [{'path': '~/PhDwiki/', 'syntax': 'markdown', 'ext': '.md', 'index': 'Home'}]
let g:vimwiki_global_ext = 0
" Install the plugin, this uses vim-plug anything else should also do
Plug 'vimwiki/vimwiki'
```

## Working with equations in the VimR preview

While it is possible to configure vim such that some symbols in equations are
replaced, this usually does not really improve the readability of the
equations. This is especially due to very limited support of most operation
such as sub- and superscripts. For revising my writing, I rely on the preview
provided by [VimR](https://github.com/qvacua/vimr). While I deactivated all
additional features of the GUI such as file browser, buffer view etc.,
features such as the markdown and html preview are the most beneficial
components of a GUI-interface compared to running NeoVim in the terminal.

Unfortunately, this preview does not support equations which are
omnipresent in courses and papers out of the box. Yet not all hope is lost, as
thanks to a nifty javascript tool [MathJax](https://www.mathjax.org/) it can be
made to do so. VimR uses a tiny browser with full javascript support
to render markdown and html pages, thus by modifying the markdown template to
include MathJax we can patch in the support of equations.

For this we need to edit the template file
`VimR.app/Contents/Resources/markdown/template.html`, by adding the following
lines just before the html tag `</head>`. The relevant region of the edited
file should look as follows:

```html
  <title>{{ title }}</title>
  <script type="text/javascript">
    window.MathJax = {
      extensions: ["tex2jax.js"],
      jax: ["input/TeX", "output/HTML-CSS"],
      tex2jax: {
        inlineMath: [ ["\\(","\\)"], ['$', '$'] ],
        displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
        processEscapes: true
      },
      "HTML-CSS": {
          availableFonts: ["STIX"],
          preferredFont: 'STIX',
          webFont: 'STIX-Web',
          imageFont: null
      }
    };
  </script>
  <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js" async></script>
</head>
```

This adds MathJax in the most light configuration to the markdown template,
allowing it to render math equations. Below you can see how the result looks:

![Vimr with markdown preview](assets/2019-04-12_VimR_equations.jpg)

While this works well in practice, it is still a rather hacky solution. For
example sometimes it is necessary to wrap the equation into a set of `<p></p>`
tags to prevent the markdown renderer from destroying the equations. To make
the approach more integrated and robust to updates of the software, I opened an
issue [here](https://github.com/qvacua/vimr/issues/718). I will edit this post
if there is any follow up development.


[^1]: I am planning to do a separate blog post on the benefits and
  disadvantages of using an editor like Vim for most of your work.
