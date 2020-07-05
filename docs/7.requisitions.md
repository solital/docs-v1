# Requisitions

## Sessions and Cookies

### Create session and cookie

The operation of the sessions and cookies are the same. To create a session, the first parameter reports the session index or the second value of it.

```php
use Solital\Session\Session;

Session::new('your_index', 'your_value');
```

And for Cookies.

```php
use Solital\Cookie\Cookie;
            
Cookie::new('your_index', 'your_value');
```

### Display session and cookie

To display a session and cookie, use a syntax below.

```php
Session::show('your_index');

Cookie::show('your_index');
```

### Delete session and cookie

To delete a session and cookie, use a syntax below.

```php
Session::delete('your_index');

Cookie::delete('your_index');
```