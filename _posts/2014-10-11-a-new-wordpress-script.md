---
date: "2014-10-11T00:00:00Z"
date_published: "2014-10-12T02:00:47.000Z"
date_updated: "2015-05-19T20:13:07.710Z"
slug: a-new-wordpress-script
title: A New WordPress Script
---

I've been working on some scripts to help with the deployment of sites to Apache lately.  My original script was pretty simple. 

I used it to automate the creation of vhost entries when I added a new site to the server.  Since one of my clients actually uses WordPress 

for all it's sites, and we have been working to migrate all the sites from a multi-site installation to stand alone sites, I needed 

an easy way to setup a Wordpress site on the new server.  Ideally I wanted to be able to pass a script the site name and it create the 

folder in `var/` , install the latest copy of WordPress, install the base themes and base plugins and setup the proper vhost entry 

in `etc/`

Ultimately I'd like to be able to incorporate this into a website so that we can install a brand new standard base install of WordPress with 

the press of a button. (I don't like how gists are embedded in jekyll so I am usinga code block + including the link to the gist)

[https://gist.github.com/weatheredwatcher/14dd0d50ee3e876746ff#file-wp_install-py](https://gist.github.com/weatheredwatcher/14dd0d50ee3e876746ff#file-wp_install-py)

        import os, sys, argparse, wget, tarfile
        if not os.geteuid()==0:
        sys.exit("\nMust be run by root\n")
    
        def main(argv):
            ##define options here
            server_root = '/var/www/vhosts/'
            plugin_path = '/var/resources/plugins/'
            theme_path = 'var/resources/themes/'
            domain = ''
            options = ''
    
            parser = argparse.ArgumentParser()
            parser.add_argument('domain')
            args = parser.parse_args(argv)
            domain = args.domain
        ##create folder    
            if not os.path.exists(server_root):
                print "Creating " + server_root
                os.makedirs(server_root)
            if not os.path.exists('tmp'):
                print "Creating Temp Folder"
                os.mkdirs('tmp')
        ##download wordpress
            download = wget.download('https://wordpress.org/latest.tar.gz')
            archive = tarfile.open(download, 'r:gz')
            archive.extractall(server_root)  
            os.rename("/var/www/vhosts/wordpress", "/var/www/vhosts/" + domain)    
        ##install base theme
        ##install base plugins
    
        ##create vhost file
            filename = domain + ".conf"
            target = open(filename, 'w')
            vhost = """
            <VirtualHost *:80>
                    ServerName %s
                    ServerAlias www.%s
                    DocumentRoot /var/www/%s
                    <Directory /var/www/%s>
                            Options -Indexes FollowSymLinks -MultiViews
                            AllowOverride All
                    </Directory>
    
                    CustomLog /var/log/httpd/%s-access.log combined
                    ErrorLog /var/log/httpd/%s-error.log
    
                    # Possible values include: debug, info, notice, warn, error, crit,
                    # alert, emerg.
            </VirtualHost>""" % (domain, domain, domain, domain, domain, domain)
            target.write(vhost)
    
        if __name__ == "__main__":
            main(sys.argv[1:])
    

My standard disclaimers apply here.  If you do not know python well enough to follow the code, I am not liable for any issues that 

might come up trying to use this code.  If you know python better then me (not a huge strectch!) please leave any comments below on 

your opinions on how to make this better!!  Also, I am happy to help anyone out who wants to use this for something and need assistance. 

My python is not by any means stellar, but I can get most things done with it!
