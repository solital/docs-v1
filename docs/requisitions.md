# Requisitions

## Sessions and Cookies

### Create session and cookie

The operation of the sessions and cookies are the same. To create a session, the first parameter reports the session index or the second value of it.

```php
use Solital\Core\Resource\Session;

Session::new('your_index', 'your_value');
```

And for Cookies.

```php
use Solital\Core\Resource\Cookie;
            
Cookie::new('your_index', 'your_value', 'time', 'path');
```

To create an array of values in a session, pass in the third parameter the name of the session index.

```php
Session::new('index', ['id' => 1, 'name' => 'hero'], 'second_index');
```


### Display session and cookie

To display a session and cookie, use a syntax below.

```php
Session::show('your_index');
Session::show('index', 'second_index');

Cookie::show('your_index');
```

### Check session and cookie

To check if a session or cookie exists, use a sintax below.

```php
Session::has('your_index');
Session::has('index', 'second_index');

Cookie::has('your_index');
```

### Delete session and cookie

To delete a session and cookie, use a syntax below.

```php
Session::delete('your_index');
Session::delete('index', 'second_index');

Cookie::delete('your_index', 'path');
```

You can use `destroy` method to destroy all sessions.

```php
Session::destroy();
```