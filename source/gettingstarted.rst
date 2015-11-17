.. index::
   single: Getting Started
   
Getting Started
========

Hello World!
~~~~~~~~~~~~~~~~

As is necessary with all applications after confirming an installation, we must write our first application! We will be making the simple "Hello World!" application with Fuchsia as a starter. It'll provide some basic incites to how routing and templates work within Fuchsia.

Routing
~~~~~~~~~~~~~~~~
By default, Fuchsia provides the following in config/routes.php

.. code-block:: php
  
  <?php
    $router->addGet('home','/')->addValues(
    	array(
    		'controller'=>'index',
    		'action'=>'index',
    		'template'=>'master',
    		'format'=>'.html'
    	)
    );
  
This is specific enough to handle only a single request and will work for our Hello World application:

.. code-block:: text

  Any request matching " / " will route to:
    A controller called indexController
    A method called index()
    The specified view in the index action will route through the "master" template
    And it will respond to a "html" format by default.
    
.. note::
  Remember, the goal of Fuchsia is to use as few dependencies as possible! ActionController\\Base should not be included for "Hello World!" unless rendering via a view.
  
However, we do not need a view to display the text "Hello World." We can remove the template and format key-value pairs for the array.

.. code-block:: php

  <?php
    $router->addGet('home','/')->addValues(
    	array(
    		'controller'=>'index',
    		'action'=>'index'
    	)
    );
  
A Controller
~~~~~~~~~~~~~~~~
Controllers are the middle-layer (business logic) of your application. The concept of the controller is to separate "Model" (database) logic from our views. Fuchsia provides an indexController that responds to any format. In the previous note we specified that we do not need ActionController\\Base. 

Given the following code in the indexController

.. code-block:: php

  <?php 
  namespace Controllers;
  
  class indexController extends \ActionController\Controller\Base
  {
  
    public function index()
    {
      $this->respondTo( function($format) {
        $format->json = true;
        $format->xml = true;
        $format->html = true;
      });
    }
    
  }
  
We can remove the inheritance from the Base Controller and any format responses and simply add:
echo "Hello World!";

.. code-block:: php

  <?php 
  namespace Controllers;
  
  class indexController
  {
  
    public function index()
    {
      echo "Hello World!";
    }
    
  }
  
