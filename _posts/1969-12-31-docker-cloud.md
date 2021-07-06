---
date: "1969-12-31T00:00:00Z"
date_published: "1970-01-01T00:00:00.000Z"
date_updated: "2018-12-19T15:47:41.337Z"
draft: true
slug: docker-cloud
title: Docker Cloud
---

I've been playing around with Docker Cloud for a little project that I have going on at work.  We are looking into using Codeship for continuous integration with this project, but we cannot use AWS.  That kinda struck down the whole EC2 Container idea! :)

This creates an issue when you want to deploy as a Docker image. Codeship can push the code to a registry but needs extra steps to deploy as a Docker container.  Triggering a new deployment by pushing to the AWS registry would have worked nicely.  Instead, I will need to push to Docker's repo and  then deploy the image from there.  This is where Docker Cloud steps in.  I can use Digital Ocean to provison a Node and then Docker Cloud can trigger a deployment whenever we push a new image to the registry.
