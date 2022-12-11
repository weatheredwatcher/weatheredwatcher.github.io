---
date: "2012-11-14T00:00:00Z"
date_published: "2012-11-14T05:00:00.000Z"
date_updated: "2012-11-14T05:00:00.000Z"
slug: using-like-to-make-better-queries
title: Using like to make better queries
---

Today I am going to impart some more command line goodness on you! (You're welcome!)\n Last time we talked about using the     like term to pull out similar tables.  Now we will use it in a query to pull out similar values from a field. 

 So here is the scenario: Let's say we are dealing with the WordPress options table.  All of our options are prefix with 'special_offers'.  So rather then create multiple queries to pull the data from each one, we will use the like operator.

    select option_name, option_value from wp_options where option_name like 'special_offers%' \G
    

This returns all of the options that are prefixed with our 'special_offers' prefix.  It's a great, easy way to pull options out of the database.

Now comes the fun part.  What about WordPress and it's built in function     get_option. True, but if you want the best performance, you don't want to make all those hits to the database.  So, lets create our own function that will let us use a more flexible mysql syntax

    function my_get_options($args)
    {
        if(!is_array($args)){$needle = $args; $operator = '=';} else {
            $needle = $args['needle'];
            $default = (empty($args['default'])) ? '' : $args['default'];
            $operator = (empty($args['operator'])) ? '=' : $args['operator'];
        }
    
    $query = "select option_name, option_value from wp_options where option_name $operator '$needle'";
    $results = mysql_query($query)or die(mysql_error());
    $num_rows = mysql_num_rows($results);
    if($num_rows == 0)
    $data = array($needle => $default);
         else {
            while($row = mysql_fetch_array($results)){
                $key = $row['option_name'];
                $value = $row['option_value'];
                $data[$key] = $value;
            }
        } 
    
    return $data;
    }
    

Yeah, that is a lot of code!  Let's go over it now.
So first of all, how do we implement this? 

    $args = array('needle' => 'special*offers%', 'operator' => 'like');
    $data = my*get*options($args);
The array sets the options (except for default, as this is really only useful for single options and not a group of options..I only included it to allow this to act as a total replacement for get*options) 

 The args are parsed by a series of tests.  If it's not an array, we assume that $args is just a string with a needle.  Then we make sure that the optional arguments exist and if not, place default values in their place.  Finally we run the query.  We check for a response, so if there is no such options, the default value can be used. Finally, we return an array with the options in a key=>value paired array!
Even though we got a little bit WordPress specific at the end, this is some good solid db stuff that you can use whenever you have a lot of data.  Remember, the less calls you have to make to the database the faster and more efficient your code will run!
