---
layout: post
title:  "Backup and sync development config files"
date:   2013-10-11 11:07:30
---

I've been working on office at days and home at nights and weekends. Duplicating development environment on different devices is boring and sometime it becomes a nightmare.
So I want the configuration files on these devices to be the same and synchronized together.

Fortunately, it can be done just via following steps:

* Use a cloud storage which provides synchronize ability on various devices.

[Dropbox](http://www.dropbox.com) and [Google Drive](https://drive.google.com/) are popular choices.

I use both.

* Store the configuration files in Dropbox or Google Drive directories.

* Make a soft link between to the directory that the development config file is placed.

The following command creates a soft link from the config file to the real one:

```bash
$ ln -s /Volumes/data/backup/Dropbox/config/php-fpm.conf /opt/local/etc/php54/php-fpm.conf
```

So all modifications I make to ```/opt/local/etc/php54/php-fpm.conf``` will be actually saved to the one in Dropbox.

These changes will be applied to my configuration file at home, and vice versa!