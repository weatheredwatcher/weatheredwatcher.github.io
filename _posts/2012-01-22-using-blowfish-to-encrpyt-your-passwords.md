---
date: "2012-01-22T00:00:00Z"
date_published: "2012-01-22T05:00:00.000Z"
date_updated: "2012-01-22T05:00:00.000Z"
slug: using-blowfish-to-encrpyt-your-passwords
title: Using Blowfish to encrpyt your passwords
---

I can't tell you how many times I've seen passwords put in databases using the md5 function that is built into php.  It is fast, easy to use and very insecure.  Granted, it's really just a last defense in case the database is accessed right?  True, but what if your users are in the majority of internet users who use the same passwords for everything?  So let's talk about using a little bit higher security on our passwords.\n

Here is a helper I wrote for Codeigniter 
[https://gist.github.com/1655410.js?file=brypt-helper.php](https://gist.github.com/1655410.js?file=brypt-helper.php)

So here is what you need to know: We are using the function crypt().  The syntax for crypt is:
    crypt(string $string, seed $seed);
Check the php references for details on the other methods of encryption.  We are only talking about bcrypt or bluefish toad 

 The way that we specify that it is bluefish that we infact want to use is in the seed itself.  We prefix it with a $2a$05$.  The first part, $2a$ is what give us the encryption method, bluefish.  The second part, $05$ sets how many passes will be used.  The more passes, the more secure, but also the longer it takes to encrypt.  The seed is the key.  By using a seed that is unique, it makes it impossible to break the hash with what is called a "rainbow table."  You make a rainbow table by compiling a dictionary of words and running them through the hash (md5, sha, bluefish) and this gives you a massive list of possible passwords and their hashes.  The adding of a seed removes the chances that the password/hash will be in a rainbow table.  The use of a seed is pretty effective in making the password unbreakable....well that is if only the hash was exposed...the seed is typically located in the table right next to the hash!
