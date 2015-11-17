.. index::
   single: Controlling Requests
   
Controlling Requests
========

Controllers are responsible for handling requests/responses. Controllers are used to retrieve data from a resource and format it appropriately for the request (generally html or json).

A Controller Serves a Request, so consider we've defined a route:

.. code-block:: php

  <?php
    $router->add('default','{/controller,action, param}')->addValues(
    	array(
    		'controller' => 'index',
    		'action' => 'index',
    		'template '=>' master',
    		'format' => '.html',
    		'param' => 'Undefined'
    	)
    );
    
Our Fuchsia Application will attempt to match any url to a controller (defaulting to indexController) and an action (defaulting to index). Otherwise a 404 error will be served (in production mode).

.. code-block:: text
  
  /index/debug
    => indexController, debug() action
    
  /home/
    => homeController, index() action
    

Passing Data to a View
~~~~~~~~~~~~~~~~
.. note::
  Fuchsia automatically creates a default route aliased as "Fuchsia.default." Your controller will look for a directory in your views with your controller prefix and a file named the same as your action.
  
A class variables defined in an action will be available to a rendered view.

.. code-block:: php

  <?php
    public function index( $name )
    {
      $this->greeting = "Hello ".$name;
      $this->respondTo( function($format) { $format->html = true; });
    }

