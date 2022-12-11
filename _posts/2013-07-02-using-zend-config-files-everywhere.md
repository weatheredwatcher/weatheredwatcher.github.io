---
date: "2013-07-02T00:00:00Z"
date_published: "2013-07-02T14:46:00.000Z"
date_updated: "2013-07-02T14:46:00.000Z"
slug: using-zend-config-files-everywhere
title: Using Zend Style Config Files Everywhere
---

It's been a while since my last blog entry, I know!!  I feel bad, so here is a bit of php goodness to make us all feel better!

It's always a good security practice to remove configuration from your web-app.  One way to do this is to use a configuration file.  Now, Zend has a very cool way of doing this,(`Zend\Config\Reader`) but the client that I am with right now is using a solution based on Codeigniter.  However, since they are planning on eventually moving to Zend anyway, I figured I would implement a solution based on the Zend config solution.

First comes the file, which I called `environment.ini` and placed in `/etc`.

The entries are in the following format:

    cg.database.name=name
    cg.database.username=username
    cg.database.password=password
    cg.database.hostname=hostname
    cg.services.name=dev
    cg.services.port=8080
    

To utilize this for the database, for example, lets create a helper with the following function:

    function get_environment(){
    $config = array();
    foreach( file( '/etc/sitename/environment.ini') as $line) {
        list( $keys, $value) = explode( '=', $line);
    
        $temp =& $config;
        foreach( explode( '.', $keys) as $key)
        {           
            $temp =& $temp[$key];
        }
        $temp = trim( $value);
    
    }
    
    return $config;
    
    }
    

This will return an array like this:

    array 
      'cg' => 
        array 
         'database' => 
            array 
              'name' => string 'name'
              'hostname' => string 'hostname'
              'username' => string 'username'
              'password' => string 'password' 
          'services' => 
            array (size=5)
              'url' => string 'dev'
              'port' => string '8080'
    

The next part of this is making use of the data in your application.  In the database config file for Codeigniter, for example:

    $dbconfig = get_environment();
    
    $db = $dbconfig['cg'][database];
    
    $db['default']['hostname'] = $db['hostname'];
    $db['default']['username'] = $db['username'];
    $db['default']['password'] = $db['password'];
    $db['default']['database'] = $db['name'];
    

And there you have it!  Of course, I based this on CI but you should be able to use this code for any framework..or even no framework.
