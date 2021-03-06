- [easyMVC](#easymvc)
  * [Requirements](#requirements)
  * [Components](#components)
    + [Controller](#controller)
    + [Actions](#actions)
    + [Views](#views)
    + [Models](#models)
      - [Configure the database access](#configure-the-database-access)
      - [Model Helpers](#model-helpers)
        * [getAll(query)](#getall-query-)
  * [How to call an action](#how-to-call-an-action)
    + [By Parameter](#by-parameter)
    + [By Route](#by-route)
      - [Advanced Routes](#advanced-routes)
  * [More Features](#more-features)
    + [Redirects](#redirects)
- [Contributors](#contributors)

# easyMVC

EasyMVC is a simple MVC framework used for teaching MVC principles.

A lot of MVC frameworks are way to complex to grasp the basic functions of an MVC framework.
easyMVC provides a simple way to teach the fundamentals of MVC frameworks without having any
dependencies or framework specific requirements.

EasyMVC **should not** be used in a productive environment.

## Requirements

- None (Should work with just a simple apache php webserver)

## Components

### Controller

All controllers **must** be created in the _/controller_ directory. They also have to extend the BaseController class.

A simple controller looks like this:
```php
public class DemoController extends BaseController {

}
```

A controllers name **must** end with _...Controller_

### Actions

Actions are methods in a controller class. The name of an action **must** end with _..Action_.

```php
public function testAction() {

}
```

Within the action it is possible to pass params to the view:
```php
public function listAction() {
    $this->view->items = [1,2,3];
}
```

### Views

A view file is a simple php file which will be rendered and sent to the client. Within the
view file you can access all variables as they are passed by the action.

Views have a defined location which is determined by the controller and action name:

_/view/**controllerName**/**actionName**.php_

So the view file of the DemoController::testAction must be inside the _demo_ Folder and be called _demo.php_:

_/view/demo/test.php_

### Models

Model classes are saved within the _model_ direcotry. All models have to extend the BaseModel class.

They allow the access the _mysqli_ instance:
```php
public class Item extends BaseModel {
    public function demoMethod() {
        $mysqli = $this->connection;    
    }
}
```

#### Configure the database access

Rename the _database_template.xml_ to _database.xml_ and add the connection data for your
mysql server.

#### Model Helpers

The following helper methods are implemented in the base model for a faster development.

##### getAll(query)
```php
public function getItems() {
    return $this->getAll("SELECT * FROM my_table");
}
```
Returns an associative array with all results from the query.

## How to call an action

### By Parameter

You can simple specify your controller and method name as GET parameters:

_easyMVC.local?controller=demo&action=test_

### By Route

easyMVC allows you to define a annotation route for every action.

To do this, add the following Code above the method declaration:
```php
/**
* @Route("/save")
*/
public function saveAction() {
}
```

This annotation allows the action to be called via the following url:

_easyMVC.local/save_

(Note that ModRewrite needs to be enabled for routes to work)

#### Advanced Routes

You can also define more than one route for each action:
```php
/**
* @Route("/save")
* @Route("/item/save")
*/
public function saveAction() {
}
```

Also you can use simple Regex expressions in your route:
```php
/**
* @Route("/save/?")
*/
public function saveAction() {
}
```

This will make the method callable via _/save_ or _/save/_.

## More Features

### Redirects

If an action should redirect the user to a different url, you can return a _Response_ Object.

```php
public function saveAction() {
    return new Response('/list');
}
```

# Contributors

- Andi Moser (andi.galileo@gmail.com)