.. index::
   single: Installation

Installation
========

The fastest way to install Fuchsia and any of its dependencies/additional packages is through Composer. Zip archives are also available for all packages via Github.

Installing Composer
~~~~~~~~~~~~~~~~
To install composer, run the following command

.. code-block:: bash

  curl -s https://getcomposer.org/installer | php
  
To validate the install, run

.. code-block:: bash

  composer about

And you should get some output text like the following:

.. code-block:: text

  Composer - Package Management for PHP
  Composer is a dependency manager tracking local dependencies of your projects and libraries.
  See https://getcomposer.org/ for more information.
  
Installing Fuchsia
~~~~~~~~~~~~~~~~
.. note::
  You may skip this if you downloaded Fuchsia and its packages via a zip directory.
  
After installing composer, navigate to your Fuchsia applications root directory. This is not the public_html directory. Your application root looks something like this:

.. code-block:: text

  application
  config
  public_html
  fuchsia
  ...
  
Simply run 

.. code-block:: bash

  composer install
  
All dependencies of Fuchsia will be installed.


Running Fuchsia
~~~~~~~~~~~~~~~~

.. note::
  This requires PHP 5.4 and the internal Webserver. This is experimental and may not work for all Fuchsia Applications. 
  
After installing Fuchsia and its packages, you can start up Fuchsia on the internal PHP webserver with the following command:

.. code-block:: bash

  php -S localhost:8080 -t public_html/ fuchsia/dev_server.php
  
Navigate your browser to localhost:8080 and see the welcome message

.. code-block:: bash

  See me in application/views/index/index.php!
  
If you wish to use Apache or Nginx, you simply need to write an htaccess rule that routes ALL requests through index.php

.. code-block:: text

  RewriteRule ^(.*)$ index.php?/$1 [L,QSA]
  
Apache with the HHVM running as a CGI service would look something like this

.. code-block:: text

  RewriteRule ^(.*)$ fcgi://127.0.0.1:9000/my/full/path/public_html/index.php?/$1 [L,P,QSA]