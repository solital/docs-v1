# Starting

Solital is a complete framework with everything you need to create high-value projects.

Solital framework is based on the [simple-php-router](https://github.com/skipperbent/simple-php-router) component, but with improvements in the core, in addition to having a cache, template, authentication, ORM system and its own console for creating pre-defined structures

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