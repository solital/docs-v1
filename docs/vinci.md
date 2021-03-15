# Vinci Console

Vinci Console is an auxiliary component to help create files faster, such as login structures, password recovery structures, database configuration and more.

## Access Vinci

To access Vinci, open the terminal in your project folder and type `php vinci [command]`

### For Windows

To run the Vinci console on Windows, remember to add the PHP directory to the Windows PATH.

To access information about Solital and its dependencies, open your terminal inside your project folder and type `php vinci about`

## Create a component
You can create a new component using the syntax below.

```
php vinci [command]:[name_file]
```

**Example**

```
php vinci controller:UserController
```

| Command      | Description                             |
|--------------|-----------------------------------------|
| `controller` | Creates a new controller                |
| `model`      | Create a new Model                      |
| `view`       | Create a new view                       |
| `css`        | Create a new CSS file                   |
| `js`         | Create a new JavaScript file            |
| `router`     | Creates a new file for the route system |

-

To see the complete list of commands, run `php vinci show`

## Remove a component

Add the `remove-` command before using one of the aforementioned commands to remove a component created with Vinci.

```
php vinci remove-controller:UserController
```

## Clearing the cache on solital

To clear the entire solital cache, run the command below.

```
php vinci cache-clear
```

## Configure database

The `db.php` file has the necessary constants for Katrina ORM to communicate with the database. To configure the `db.php` file, run the command` php vinci katrina:configure`

This command will ask for: database drive, host, database name, database user and password. Remember to enter the values in that order as shown below.

```shell
Enter the drive, host, database name, username and password for your database separated by commas


> mysql, localhost, db_solital, root, root
```

If you want to create a standard user in the database, run `php vinci katrina:userAuth`

**Custom commands**

You can create a custom method to create your tables using the `SQLCommands` method.

```php
<?php

namespace Solital\Database\Create;
use Solital\Components\Model\Model;
use Solital\Database\Create\Create;

class SQLCommands extends Model
{
    public function myTable()
    {
        # Here will be the commands for creating a table
    }
}
```

And in vinci, execute the method as follows:

```
php vinci katrina:myTable
```

## Login structure

To create a predefined login structure, use `php vinci auth`

This command will create a `LoginController` class, templates for authentication and dashboard and predefined routes. Plus a standard user in the database. To learn more visit [this link](https://solital.github.io/docs-v1/auth).

If you want to remove this structure, use `php vinci remove-auth`

**NOTE:** the command to remove the components does not remove the routes created.

## Recover password structure

You can create a predefined password recovery framework. To do so, use the `php vinci forgot` command

This command creates a controller with the name `ForgotController`. With it you will have all the basis to create a password recovery system. To learn more visit [this link](https://solital.github.io/docs-v1/security).

**NOTE:** the command to remove the components does not remove the routes created.