---
date: "2013-02-01T00:00:00Z"
date_published: "2013-02-01T05:00:00.000Z"
date_updated: "2013-02-01T05:00:00.000Z"
slug: preparing-a-dev-environment-with-puppet
title: Preparing a Dev Environment with Puppet
---

For starts, I now have markup installed in my blog, so no more typing html!! Yea!\n 

Today we are going to talk about Puppet.  No, not Pinochio, or those Punch and Judy dolls.  This is Puppet as in the server provisioning tool.

At work I am setting up a development environment for our dev team.  Since most of them are just learning php, and for over all consistency I am using Vagrant to build a standard dev vm for everyone to work off of.

The general requirements are simple:

1. We must run Zend Server  
2. We must load the php drivers MS Sql  
3. We must install subversion

With these requirements in mind, I set out to build my first puppet script.

The first class that we define is our services class.  I need to make sure that Apache is running.  Also, I found out that Cent Os turns iptables on by default.  That interferes with the dev box, as well as being unnecessary!  So we make sure that iptables is off.

    class services {  
      #we want apache
      service {
        'httpd':
          ensure => running,
          enable => true
      }
    
    service {  
      'iptables':
        ensure => stopped,
        enable => false
     }
    }
    

The next two classes work in tandem.  The repos class defines our Zend Server repo and packages install the required packages.

    class packages {  
      package {
        "httpd":                      ensure => "present"; # Apache
        "subversion":                 ensure => "present"; # Subversion
        "zend-server-ce-php-5.3":     ensure => "present"; # Zend Server (CE)
        "php-5.3-mssql-zend-server":  ensure => "present"; # MSSQL Extenstion - provided by Zend
      }
    }
    
    
    class repos {  
      #lets install some repos
      file { "/etc/yum.repos.d/zend.repo":
        content => "[Zend]
        name=Zend Server
        baseurl=http://repos.zend.com/zend-server/rpm/x86_64
        enabled=1
        gpgcheck=1
        gpgkey=http://repos.zend.com/zend.key
    
        [Zend_noarch]
        name=Zend Server - noarch
        baseurl=http://repos.zend.com/zend-server/rpm/noarch
        enabled=1
        gpgcheck=1
        gpgkey=http://repos.zend.com/zend.key
        "
      }
    
    }
    

If anyone wants to see the entire file, here it is:

    stage {
    
      'users':      before => Stage['repos'];
      'repos':      before => Stage['packages'];
      'packages':   before => Stage['configure'];
      'configure':  before => Stage['services'];
      'services':   before => Stage['main'];
    
    }
    
    class services {  
      #we want apache
      service {
        'httpd':
          ensure => running,
          enable => true
      }
    
      service {
        'iptables':
          ensure => stopped,
          enable => false
      }
    }
    
    class configure {
    
      # symlinking the code from /home/vagrant/public to var/www/public
      exec { "public simlink":
        command => "/bin/ln -s /home/vagrant/public /var/www/",
        unless  => "/usr/bin/test -L /var/www/",
      }
      file {"/var/www/index.html":
        ensure => "absent"
    
      }
    }
    
    class packages {  
      package {
        "httpd":                      ensure => "present"; # Apache
        "subversion":                 ensure => "present"; # Subversion
        "zend-server-ce-php-5.3":     ensure => "present"; # Zend Server (CE)
        "php-5.3-mssql-zend-server":  ensure => "present"; # MSSQL Extenstion - provided by Zend
      }
    }
    
    class repos {
    
      file { "/etc/yum.repos.d/zend.repo":
        content => "[Zend]
    name=Zend Server  
    baseurl=http://repos.zend.com/zend-server/rpm/x86_64  
    enabled=1  
    gpgcheck=1  
    gpgkey=http://repos.zend.com/zend.key
    
    [Zend_noarch]
    name=Zend Server - noarch  
    baseurl=http://repos.zend.com/zend-server/rpm/noarch  
    enabled=1  
    gpgcheck=1  
    gpgkey=http://repos.zend.com/zend.key  
        "
      }
    
    }
    
    class users  
    {
      group { "puppet":
        ensure => "present",
      }
      user { "vagrant":
        ensure => "present",
    
      }
    }
    
    class {  
      users:      stage => "users";
      repos:      stage => "repos";
      packages:   stage => "packages";
      configure:  stage => "configure";
      services:   stage => "services";
    
      }
    
