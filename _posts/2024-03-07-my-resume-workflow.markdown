---
date: "2024-03-07"
date_published: "2024-03-07"
date_updated: "2024-03-07"
slug: "my-resume-workflow"
title: My Resume Workflow
tags: [dev, rake, ruby]
type: "post"
---
I started writing my Resume in Markdown a while ago.  I started out just using Pandoc to convert it to pdf.  Overtime, I started my own little
build process going.  I use rake.

Originally, I setup with targets specifically for PDF and DOCX. I did this because although most of the time sending in a PDF is good enough,
occasionally someone asks for it as a Word Doc.

Eventually, I felt the need of customizing the resume between Full Stack and Platform roles that I was seeking.  To make this work, I have divided into
headers that a specific to the various roles, as well as a base resume file. Then, I just combine them as required in Pandoc with different Rake tasks.

My rakefile looks like this:

```ruby
task :default => :pdf

task :pdf => [:fs, :pe, :as]

task :fs do
  sh "pandoc header-fs.md resume-base.md -s -o duggins-fs-resume.pdf"
end

task :pe do
  sh "pandoc header-pe.md resume-base.md -s -o duggins-pe-resume.pdf"
end

task :as do
  sh "pandoc header-as.md resume-base.md -s -o duggins-as-resume.pdf"
end

task :html do
  sh "pandoc resume.md -s -o duggins-resume.html --metadata pagetitle=\"David Duggins Resume\""
end

task :docx do
  sh "pandoc header-fs.md resume-base.md -s -o duggins-fs-resume.docx"
  sh "pandoc header-pe.md resume-base.md -s -o duggins-pe-resume.docx"
end
```

Basically, when I generate a pdf, it creates one of each of the types.  So when I update my resume, I regenerate the pdf's and it sets them up for all of the variants at once.

A few things that I am aiming to do in the future would be to also sperate the section building out and be able to generate either a pdf or a Word Doc more dynamically and the other would be
to add a date to the titles and maybe add a cleanup function that would only keep the last 2-3 versions of the resume on hand.
