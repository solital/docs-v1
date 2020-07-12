# Authenticate

Solital uses the `Guardian` class to perform authentication. First of all, make sure to change the Guardian constants in config.php inside the config folder.

```php
/**
* GUARDIAN CONSTANTS
*/

/* Login route if login verification is false */
define('URL_LOGIN', 'your_login_url');
/* Dashboard route if login verification is true */
define('URL_DASHBOARD', 'your_dashboard_url');
/* Standard Guardian index */
define('INDEX_LOGIN', 'solital_index_login');
```

## Setting
On your Controller, extend the `AuthController` class.

```php
<?php

namespace Solital\Components\Controller;
use Solital\Components\Controller\Auth\AuthController;

class UserController extends AuthController {
    #...
}
```

To do this, define your database username and password fields, the values ​​of your form input and the table where your username and password will be stored as shown below.

```php
$res = $this->columns('your_username_column', 'your_password_column')
            ->values('input_username_name', 'input_password_name')
            ->register('your_table');
```

The `$res` variable will return `true` if authentication is true. But if it is `false`, you can add a reply message after the above code if authentication fails.

```php
if ($res == false) {
    Message::newMessage('wrongLogin', 'Incorrect username or password');
    response()->redirect('your_login_url');
}
```

Below is an example method of authentication.

```php
<?php

namespace Solital\Components\Controller;
use Solital\Components\Controller\Auth\AuthController;

class UserController extends AuthController 
{

    public function verifyLogin() 
    {
        $res = $this->columns('email_column', 'pass_column')
                    ->values('input_email', 'input_password')
                    ->register('tb_users');

        if ($res == false) {
            Message::newMessage('wrongLogin', 'Incorrect username or password');
            response()->redirect('your_login_url');
        }
    }

}
```

## Check login

To ensure that the user is authenticated, use `checkLogin()` method. If the login has not been validated using the `validate()` method, the user will be redirected to the route defined in the constant `URL_LOGIN`. The example below shows the method along with the Wolf template.

```php
Guardian::checkLogin();
            
Wolf::loadView('home');
```

To ensure that the user doesn't fall into the login route when it has already been validated, insert the `checkLogged()` method in your login route. This method will redirect the user to your system's dashboard. Make sure your constant `URL_DASHBOARD` has a defined value.

```php
Guardian::checkLogged();

Wolf::loadView('login');
```

## Log off
To log off, use the `logoff()` method.

```php
Guardian::logoff();
```