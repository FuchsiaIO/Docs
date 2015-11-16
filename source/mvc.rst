.. index::
   single: MVC Pattern
   
MVC Pattern
========

Routes
------------------
Routes handle how a specific URL is sent through your application. By default, all requests, excluding various file extensions, will be sent through index.php. Take the following request for example:
.. code-block:: bash
  http://fuchsia.io/blog/
  
Unless otherwise specified in routes.php, Fuchsia will attempt to route to a blogController and its index action.

Controllers
------------------
Controllers are the middle-layer (business logic) of your application. The concept of the controller is to separate "Model" (database) logic from our views. It is here where you can specify the response types you want your controller to respond to (html, json, xml, etc) and/or the view you would like to render.

Models
------------------
Models are responsible for creating, reading, updating, and deleting (CRUD) records from your database.

Views
------------------
Views format the data received from the controller. Generally with HTML/HAML