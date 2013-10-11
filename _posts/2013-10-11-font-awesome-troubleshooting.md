---
layout: post
title:  "Font Awesome troubleshooting"
date:   2013-10-11 13:21:25
---

[Font Awesome](http://fortawesome.github.com/Font-Awesome) is popular font icon library. This article introduces most
popular Font Awesome troubleshooting with the solutions.

> The commands in this post are used on MacOS. It might be a little bit different on other operating systems.

## Font Awesome does not work on Firefox

Firefox does not show the icon created by Font Awesome if the Font Awesome is hosted in other asset, CDN server that is not the same
as main web server.

Some CDN providers store the Font Awesome library, such as [Cloudflare](http://cdnjs.cloudflare.com/ajax/libs/font-awesome/3.2.1/css/font-awesome.css),
[Bootstrap CDN](http://netdna.bootstrapcdn.com/font-awesome/3.2.1/css/font-awesome.css), etc.

The reason is that Firefox prevents itself from loading fonts from other domains.

To fix this issue, do the following steps:

**For nginx web server**

* Add the following ```location``` section to ```server``` setting section:

    ```
    server {
        location ~* \.(eot|ttf|woff)$ {
            add_header Access-Control-Allow-Origin *;
        }
    }
    ```

* Restart nginx webserver:

    ```bash
    $ sudo nginx -s reload
    ```

**For Apache web server**

* Add the following lines to Apache configuration file (```apache2.conf```, for example):

    ```
    <FilesMatch ".(ttf|otf|woff)$">
        Header set Access-Control-Allow-Origin "*"
    </FilesMatch>
    ```

* Restart Apache web server:

    ```bash
    $ sudo apachectl restart"
    ```