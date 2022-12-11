---
date: "2013-08-07T00:00:00Z"
date_published: "2013-08-07T14:46:00.000Z"
date_updated: "2013-08-07T14:46:00.000Z"
slug: devops-building-projects-with-ant
title: 'DevOps: Bulding Projects with Ant'
---

I've been running a build process for an application that I am working on that is rather complicated.  Originally I was manually managing the build process, with a few scripts to supplement my procedures.  The past few days I have finally had the time to sit down and consolidate my build into a single ant script.  The company that I am contracting for is a heavy Java shop, so I am using Ant mostly because everyone will have access to ant.  I am use to make so it's been a bit of a learning curve, but Ant is a very cool thing.  So, for starters, let me outline my current build process and then I will show you how I have automated it.

1. We pull the code from the QA branch in GitHub  
2. Run Composer over the code to install all the dependencies  
3. We tag the build with a version tag  
4. We write the version tag and the date to a version file  
5. We tar up ONLY the files that are absolutely needed to run the application (we have tools, docs, ect that we do not need)  
6. We send the final archive to the server team for deployment

Actually, I started using Ant early on for the final tar build.  Ant has a great task for tar.  Here is the target for that.

    <target name="deploy">
          <tar destfile="deploy.tar"
               basedir="build/"   
               excludes="build/**, database/**, docs/**.vagrant/**"/>
    
    </target>
    

This essentially creates a file called `deploy.tar` in a sub-folder called `build/`.  It excludes anything already in `build/` as well as `database/`, `docs/', and`.vagrant'.  Obviously I truncated my list...it is a lot longer in reality!  A lot of files sit in the root like the `Vagrantfile`, a `Makefile` the files for composer.

So this is about the final step.  But I still need to automate the rest.

The first step is grabbing my code from GitHub.  Eclipse offers a task for any that is based on jgit.  You need a few dependencies for this to work.  Namely the jgit class file and the jgit-ant class file.  You also need the ssh library, which I was already using for some other scripts.

You need to load them as resources, which is done like so:

    <taskdef resource="org/eclipse/jgit/ant/ant-tasks.properties">
         <classpath>
           <pathelement location="resources/org.eclipse.jgit.ant-3.0.0.2013061825-r.jar"/>
           <pathelement location="resources/org.eclipse.jgit-3.0.0.2013061825-r.jar"/>
           <pathelement location="../jsch-0.1.49.jar"/>
         </classpath>
    </taskdef>
    

Then we set up the task for cloning:

    <target name="clone">
         <git-clone 
             uri="git@github.com:weatheredwatcher/weatheredwatcher.git" 
             branch="testing"
             dest="build/" />
    </target>
    

(Note that I am using this site rather then the project that I am working on...)

The next step is running Composer.  For those that do not know, Composer is a dependency management tool for PHP.  We are loading several dependencies as well as some custom libs via composer so it is important that we generate the right files.

    <target name="init" description="Installing Denpendencies">
      <delete file="build/composer.lock" />
    
        <exec executable="php" failonerror="true"
            dir="build/">
            <arg value="composer.phar" />
            <arg value="install" />
      </exec>
    
    </target>
    

So what we are doing here is first we delete the lock file. Typically the devs install a few extra tools that are not needed on the QA server.  So we remove the lock file and then only install the production level requirements with Composer.  `failonerror` ensure that we get an error if anything bad happens rather then a success.

As far as tagging goes, I feel it is better to do the tagging in GitHub rather then in the build process.  So we will only be writing to a version file.  We need to write the current tag as well as the build date to this file.  The git command for displaying the current tag is `git describe --exact-match --abbrev=0`.  We antify this like so:

    <exec executable="git" failonerror="true"
       dir="build/">
       <arg value="describe" />
       <arg value="--exact-match" />
       <arg value="--abbrev=0" />
       <redirector output="build/version" />
    </exec>
    

The last part was hard.  You cannot pass a redirect `>` through the `exec` task.  Instead, we use the `redirector`.  The date is similar, but we add an append option to the redirector to make sure we do not overwrite the file.

    <exec executable="date" dir="build/">
      <redirector output="build/version" append="true"/>
    </exec>  
    

So if we put it all together into a massive ant script, we have nearly the entire deploy build.  The last part is the one where we send it along to the Server Team's folders via a mount and a copy.

The final thing to do is to clean up.  

    <target name="clean">
        <delete dir="build"/>
    </target>
    

My next step will be taking this process and integrating it into a Hudson build for Continuous Integration or CI.  Obviously, Hudson will be able to take on a lot of this functionality without any ant scripts...but I also can just have Hudson run the ant script if I want.  We will see.  Until then, happy coding!!
