# Dependecy Container (PSR-11)

Solital has implemented the PSR-11, that is, you can create containers easily and with good practices.

The syntax below shows a clear example.

```php
use Solital\Core\Course\Container\Container;

$container = new Container();

$container->set('user', function($args) {
    return new UserModel($args);
}, new ContactModel());
```

And to retrieve a value:

```php
$dep = $container->get('user');
$dep->run();
```

You can also use containers within the classes.

```php
<?php

namespace Solital\Components\Controller;
use Solital\Components\Model\UserModel;
use Solital\Components\Model\ContactModel;
use Solital\Core\Course\Container\Container;

class UserController
{
    private $container;

    public function __construct()
    {
        $this->container = new Container();

        $this->container->set("user", function($args) {
            return new UserModel($args);
        }, new ContactModel());
    }

    public function user()
    {
        $dep = $this->container->get('user');
        $dep->run();
    }

    public function container()
    {
        $dep = $this->container->get('user');
        $dep->run();
    }
}
```