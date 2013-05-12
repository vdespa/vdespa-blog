# How to Change php.ini Values Using .htaccess #

You can find yourself often in the situation when you have to change the `php.ini` configuration on a server where you don't have full control (like no root, ssh) and you only can upload files to that server. Getting access to the server configuration may be difficult in this situation. Sending an email to the customer, explaining what is needed, of course he has no idea what you are talking about, trying to get in touch with the support from different hosting companies and so on - what a waste of time.

<!--BREAK-->


## Servers supported ##

I've tested this method on Apache and on Zend Server. For Apache the [AllowOverride Directive](http://httpd.apache.org/docs/2.2/mod/core.html#allowoverride) must be properly configured.

I am pretty sure your server must NOT run PHP in "CGI mode" for this to work.

## Basic examples ##

Practically the setting will be set with `php_flag` or `php_value`

### Error handling ###

**Show errors**

    php_flag display_errors {on / off}

**Set error reporting level**

    php_value error_reporting {value}

The `value` is an integer and to get `value` you want you may want to do something like:

    <?php
    echo E_WARNING & ~E_NOTICE; // 2
    echo E_ALL; // 32767



## Other alternatives ##

From your PHP script you may also set php.ini values using the `ini_set()` function.

#### References ####

- [How to Override PHP Configuration Options](http://www.sitepoint.com/how-to-override-php-configuration-settings/)


