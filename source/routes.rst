.. index::
   single: Basic Routing
   
   
Basic Routing
========

Routes are the backbone of your application. The router is in charge of parsing a requested URL and routing it to the appropriate controller/action. Fuchsia uses a Fork of AuraPHP's router.

Instantiation
~~~~~~~~~~~~~~~~
.. note::
  Fuchsia, generally, only needs a single router. Route namespaces are preferred over multiple route-instantiations.
  
Instantiate a new router like so:

.. code-block:: php

  <?php
    use \ActionDispatch\Dispatcher as Dispatcher;
    $router = (new Dispatcher\Factory\RouterFactory)->newInstance();
    
Adding Routes
~~~~~~~~~~~~~~~~
.. note::
  The order in which you define your routes does affect how your application executes. The router will match the URL to the first match, not necessarily the "best" match.
  
To create a route, call the add() method on the Router. Named path-info params are placed inside braces in the path.

.. code-block:: php

  <?php
  // add a simple named route without params
  $router->add('home', '/');
  
  // add a simple unnamed route with params
  $router->add(null, '/{controller}/{action}/{id}');
  
You can create a route that matches only against a particular HTTP method as well. The following Router methods are identical to add() but require the related HTTP method:

.. code-block:: php
  
  <?php
    $router->addHead()
    $router->addGet()
    $router->addDelete()
    $router->addOptions()
    $router->addPatch()
    $router->addPost()
    $router->addPut()

Generating A Route Path
~~~~~~~~~~~~~~~~
To generate a URL path from a route so that you can create links, call generate() on the Router and provide the route name with optional data.

.. code-block:: php

  <?php
  // $path => "blog/read/42"
  $path = $router->generate('read', array(
      'id' => 42
  ));
  
Route Groups
~~~~~~~~~~~~~~~~
You can add a series of routes all at once under a single "mount point" in your application. For example, if you want all your blog-related routes to be mounted at /blog in your application, you can do this:

.. code-block:: php

  <?php
  $name_prefix = 'blog';
  $path_prefix = '/blog';
  
  $router->attach($name_prefix, $path_prefix, function ($router) {
  
      $router->add('browse', '{format}')
          ->addTokens(array(
              'format' => '(\.json|\.atom|\.html)?'
          ))
          ->addValues(array(
              'format' => '.html',
          ));
  
      $router->add('read', '/{id}{format}')
          ->addTokens(array(
              'id'     => '\d+',
              'format' => '(\.json|\.atom|\.html)?'
          ))
          ->addValues(array(
              'format' => '.html',
          ));
  
      $router->add('edit', '/{id}/edit{format}')
          ->addTokens(array(
              'id' => '\d+',
              'format' => '(\.json|\.atom|\.html)?'
          ))
          ->addValues(array(
              'format' => '.html',
          ));
  });
  
Route Namespaces
~~~~~~~~~~~~~~~~
Route Namespaces are essentially the same as a route group with one exception: it enforces that controllers be in a nested directory matching the namespace. For example, you could do something like the following:

.. code-block:: php

  <?php
    $router->setNamespace('v1','/api/v1',function($route){
      
      $route->addValues(array(
        'controller' => 'user',
        'action'     => 'current_user'
      ));
       
    },array('format'=>'.json'));

This will enforce that you have a directory in application/controllers called v1. From here all controllers within this namespace would look something like the following:

.. code-block:: php

  <?php
    namespace Controllers\v1;
    class userController extends apiController
    {
      public function current_user()
      {
        // some processing logic
      }
    }
    
You are also able to specify some default values in the second parameter as a hash. Valid options include:

* directory
* format
* controller
* action
* template