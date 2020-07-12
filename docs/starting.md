# Starting

Solital is a complete framework with everything you need to create high-value projects.

## Installing via Composer 

To download Solital, use the command below:


```php
composer create-project solital/solital [solital_project_folder]
```
        
It only takes a few lines of code to get started:

```php
Course::get('/', function() {
    return 'Hello world';
});
```
        
## Running
To execute the project, use the built-in PHP server or create a virtual host:

```php
php -S localhost:8000 -t public/
```
        
## Requirements

- PHP 7.1 or greater
- PHP JSON extension enabled
- PHP PDO extension enabled

## Features

- Basic routing (GET, POST, PUT, PATCH, UPDATE, DELETE) with support for custom multiple verbs.
- Regular Expression Constraints for parameters.
- Wolf template system.
- HTTP client manipulation.
- Vinci development assistant.
- Middleware (classes that intercepts before the route is rendered).
- ORM for database persistence
- CSRF protection.
- Sub-domain routing
- Custom boot managers to rewrite urls to "nicer" ones.
- Input manager; easily manage GET, POST and FILE values.