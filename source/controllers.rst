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
    
Specifying a View
~~~~~~~~~~~~~~~~
.. note::
  Fuchsia automatically creates a default route aliased as "Fuchsia.default." Your controller will look for a directory in your views with your controller prefix and a file named the same as your action.
  
If you wish to overload the Fuchsia.default view, you may specify a view alias via the setView() method:

.. code-block:: php

  <?php
    //indexController...
    public function home()
    {
      // load views/index/index.php (or index.haml.php)
      // instead of views/index/home.php
      $this->setView('index.index');
    }
    
Overloading the Template
~~~~~~~~~~~~~~~~
  
If you wish to overload the default (master) template, you may specify a template via the setTemplate() method:

.. code-block:: php

  <?php
    public function error()
    {
      // specify the error template alias instead of master
      $this->setTemplate('error');
    }

Passing Data to a View
~~~~~~~~~~~~~~~~
  
A class variables defined in an action will be available to a rendered view.

.. code-block:: php

  <?php
    public function index( $name )
    {
      $this->greeting = "Hello ".$name;
      $this->respondTo( function($format) { $format->html = true; });
    }

Filtering Actions
~~~~~~~~~~~~~~~~
Action filters are controller methods that will execute before or after any specified requests. There are two filters available:

* before_filter
* after_filter

Each of these filters can have a specified action, containing two options:

* except
* only

Except option will allow the filter to execute against all actions except the ones specified.
Only option will execute the filter against only the specified actions.

The following example will execute the action "require_login" for all actions except: index, login, unauthorized.

.. code-block:: php

  <?php
    // controller declaration...
    
    static $before_filter = array(
      'require_login' => [
        'except' => array('index','login','unauthorized')
      ] 
    );
    
    public function index()
    {
      // will not have require_login execute
    }
    
    public function me()
    {
      // require_login will execute before this action
    }
    
    private function require_login()
    {
      // the filter action
      // check login state, otherwise redirect ...
    }

Default Responses
~~~~~~~~~~~~~~~~
If you wish to have/enable a default response for all controller actions, you may do so in the constructor of the controller. There is no need to call parent::__construct since the Base controllers constructor is empty.

.. code-block:: php

  <?php
  
    public function __construct()
    {
      $this->respondTo( function($format) { $format->html = 'fuchsia.default'; });
    }
