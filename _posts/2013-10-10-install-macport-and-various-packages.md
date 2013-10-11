---
layout: post
title:  "Install MacPort and various packages"
date:   2013-10-10 20:16:42
---

## What is MacPort?

There are two ways to install a library, package on MacOS:

* __Manually__: Using a compiler

    Compile the package. Most of the packages are written in C language, therefore using a C/C++ compiler is a general way to compile, and install these packages.
    If the package requires other dependencies one, you have to off course download, compile them manually.

* __Automatically__: Using packages manager

    The package managers help you install any package automatically, easily. It also download and install the required packages automatically. Other operating systems provide manager like this, such as _yum_ (on Centos), _apt_ (on Ubuntu).
    On MacOS, there are [MacPort](http://www.macports.org), [Homebrew](http://mxcl.github.io/homebrew), and [Fink](http://www.finkproject.org).

## Install MacPort

* Download and install the latest version of [XCode](https://developer.apple.com/xcode/)
* Launch XCode from _Application_ directory, choose _XCode &rarr; Preferences_.
Click the _Downloads_ tab, and click the _Install_ button associating with the _Command Line Tools_ section.
* Accept the Xcode license by running the following command in the Terminal:

    ```bash
    $ sudo xcodebuild -license
    ```

    Press _space_ bar till the end of license content. Type _agree_ to accept the license.

* Download the latest version of [MacPort](http://www.macports.org/install.php). At the time of writing, the latest version 2.1.3 is available for [Mountain Lion](https://distfiles.macports.org/MacPorts/MacPorts-2.1.3-10.8-MountainLion.pkg).
Double-click the downloaded file and follow the install instruction.

![Install MacPort](/img/install-macport.png)

## Install various packages

To use MacPort, first, you have to add _port_ containing directory to ```$PATH``` environment variable by running the command below:

```bash
$ export PATH=/opt/local/bin:/opt/local/sbin:$PATH
```

The following commands compare the syntax of installation command in MacPort, yum and apt (where ```PackageName``` is the name of package)

Package manager | Installation command
----------------|---------------------
yum             | ```$ yum install PackageName```
apt			    | ```$ apt-get install PackageName```
MacPort		    | ```$ sudo port install PackageName```

It is quite easy, right?. The next sections show you how to install and configure most popular packages for your developing environment.

### Install PHP

*PHP 5.4*

Execute the command:

```bash
$ sudo port install php54
```

*PHP-FPM*

* Execute the command:

    ```bash
    $ sudo port install php54-fpm
```

* Create the php-fpm configuration file from a sample one:

    ```bash
    $ cd /opt/local/etc/php54/
    $ sudo cp php-fpm.conf.default php-fpm.conf
    ```

* Edit the php-fpm configuration file (```/opt/local/etc/php54/php-fpm.conf```):

```pid = run/php54/php-fpm.pid```

From now on, you can start, stop php-fpm with the following commands:

| Function        | Command
| ----------------|--------
| Start php-fpm   | ```$ sudo /opt/local/sbin/php-fpm54```
|				  | php-fpm can be set to run automatically when the system starts
|				  | by executing the following command:
|				  | ```$ sudo port load php54-fpm```
| Stop php-fpm    | ```$ sudo kill 'cat /opt/local/var/run/php54/php-fpm.pid'```
| Restart php-fpm | Run the stop and start commands, respectively.

*PHP extensions*

```bash
$ sudo port install php54-curl php54-exif php54-gd php54-imagick php54-intl
php54-mbstring php54-mcrypt php54-mongo php54-mysql php54-pcntl php54-redis
```

Remember that __after installing any PHP extensions, you have to restart php-fpm, not your web server (nginx, apache, etc.)__.

### Install Nginx

* Run the below command:

    ```bash
    $ sudo port install nginx
    ```

* Create the configuration files from sample one:

    ```bash
    $ cd /opt/local/etc/nginx/
    $ sudo cp fastcgi_params.example fastcgi_params
    $ sudo cp mime.types.example mime.types
    $ sudo cp nginx.conf.default nginx.conf
    ```

* Start Nginx:

    ```bash
    $ sudo nginx
    ```

If you see _Welcome to nginx!_ message when navigating to <http://localhost>, then Nginx is installed successfully.

![Welcome to Nginx](/img/nginx.png)

### Test PHP with Nginx

* Edit the nginx configuration:

    ```bash
    $ sudo nano /opt/local/etc/nginx/nginx.conf
    ```

    Modify the content of file as following:

    ```
    http {
        ...
        server {
            ...
            location / {
                root   share/nginx/html;
                index  index.html index.htm;
            }
            location ~ \.php$ {
                root                        share/nginx/html;
                fastcgi_pass                127.0.0.1:9000;
                fastcgi_index               index.php;
                fastcgi_intercept_errors    on;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
            }
            ...
        }
        ...
    }
    ```

* Restart nginx:

    ```bash
    $ sudo nginx -s reload
    ```

* Create a new file named ```phpinfo.php```, located at the ```/opt/local/share/nginx/html``` directory, with the following content:

    ```php
    <?php
    phpinfo();
    ```

* Point the web browser to <http://localhost/phpinfo.php> to see the information of PHP installed on the server.

![PHP Information](/img/phpinfo.png)

### Install MongoDB

* Install mongodb package:

    ```bash
    $ sudo port install mongodb
    ```

* Run the following command to start MongoDB and cause it to launch startup:

    ```bash
    $ sudo port load mongodb
    ```

* Check the MongoDB installation:

    ```bash
    $ mongo
    MongoDB shell version: 2.4.3
    connecting to: test
    ```

### Install Redis

* Install redis package:

    ```bash
    $ sudo port install redis
    ```

* Start the Redis server at startup:

    ```bash
    $ sudo port load redis
    ```

## Use MacPort GUI

If you wish to use a GUI application for managing MacPort package, then [Pallet](http://trac.macports.org/wiki/MacPortsGUI) is right option for you. Run the following command to install Pallet:

```bash
$ sudo port install Pallet
```

Pallet then is installed and located at _Applications/MacPorts_ directory.