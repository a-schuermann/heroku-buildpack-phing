Apache+PHP with Phing Build Pack for Symfony2 on Cloudcontrol
================================

This is a fork of [Apache+PHP with Phing Build Pack](https://github.com/ryanbrainard/heroku-buildpack-phing), which is a fork of [PHP build pack](https://github.com/heroku/heroku-buildpack-php).
It is created to use with [cctrl](http://www.cloudcontrolled.com/), but may be used with any other Plattform.
Feel free to fork it to make your desired changes.
Also feel free to contact me if you have any ideas to improve this fork.

Usage
-----

Repositories should have a Phing build file called `build.xml` in their root directory with a `stage` target that
deposits files to the "target" directory and also calls the "build" target which can do all the other work like i.e.
updating the database, warming up the cache etc.tt.
An example for symfony2 projects is attatched with this fork.

Configuration
-------------

The config files are bundled with the build pack itself:

* conf/httpd.conf
* conf/php.ini


Pre-compiling binaries
----------------------

    # apache
    mkdir /app
    curl --location -O http://www.gtlib.gatech.edu/pub/apache//httpd/httpd-2.2.22.tar.gz
    tar xvzf httpd-2.2.22.tar.gz 
    cd httpd-2.2.22
    ./configure --prefix=/app/apache --enable-rewrite --enable-expires --enable-headers --disable-setenvif     
    make
    make install
    cd ..
    
    # php
    wget http://us2.php.net/get/php-5.3.15.tar.gz/from/us.php.net/mirror 
    mv mirror php.tar.gz
    tar xzvf php.tar.gz
    cd php-5.3.15/
    ./configure --prefix=/app/php --with-apxs2=/app/apache/bin/apxs --with-mysql --with-pdo-mysql --with-pgsql --with-pdo-pgsql --with-iconv --with-gd --with-curl=/usr/lib --with-config-file-path=/app/php --enable-soap=shared --with-openssl --enable-cli --with-readline --enable-pcntl --with-mcrypt  --enable-shared=no --enable-static=yes
    make
    make install
    cd ..
    
    # php extensions
    mkdir /app/php/ext
    cp /usr/lib/libmysqlclient.so.16 /app/php/ext/
    cp /usr/lib/libmcrypt.so.4 /app/php/ext/
    
    # package
    cd /app
    echo '2.2.22' > apache/VERSION
    tar -zcvf apache-2.2.22.tar.gz apache
    echo '5.3.15' > php/VERSION
    tar -zcvf php-5.3.15.tar.gz php

Meta
----

Apache+PHP with Phing Build Pack was originally Created by Pedro Belo and extended for Phing by Ryan Brainard.
This Fork for sf2 on cctrl was created by Ansgar Schuermann.
