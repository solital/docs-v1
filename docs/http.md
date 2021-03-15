# HTTP CLient

Solital has a component to manipulate the HTTP client, useful for consuming API and making HTTP requests.

Before, you will need to install the component using the command below:

```
composer install solital/http-client
```

## Requirements

- PHP >= 7.2 (Compatible with PHP 8)
- CURL extension enabled

## Basic use

To make basic use of the component, you will need to instantiate the `HttpClient` class. Then, call the `request` function, passing the method (GET, POST, ...) and the url as a parameter. Then return the data.

```php
use Solital\HttpClient;

$client = new HttpClient();
$client->request("GET", "http://api.url.com");
$res = $client->toJson();

pre($res);
```

## Supported methods

HttpClient supports the methods below.

- GET
- POST
- PUT
- DELETE
- PATCH
- OPTIONS

## Return types

You can return the answer in json, array or object. The methods below show an example.

```php
$client = new HttpClient();
$client->request("GET", "http://api.url.com");

/** Return json */
$res = $client->toJson();

/** Return array */
$res = $client->toArray();

/** Return object */
$res = $client->toObject();

pre($res);
```

## Sending data

If you are using a POST or PUT request, for example, and need to send data to the HTTP header, you can pass an array with the values in the `request` method.

```php
$client = new HttpClient();

$res = $client->request("PUT", "http://api.url.com", [
    'data' => 'your_data_values'
])->toJson();

echo $res;
```

## Custom Headers

By default, HttpClient has the following headers:

- Content-Type: application/json
- Accept: application/json

To add other headers to the request, use an array in the constructor on the instance.

```php
$headers = [
    'Content-Type: application/pdf'
];

$client = new HttpClient($headers);
#...
```

## Enabling SSL verification

HttpClient by default does not perform SSL verification. To enable verification, use the `enableSSL` method.

```php
$client = new HttpClient();
$client->enableSSL();
#...
```

## Authentication

### Basic Auth

To perform authentication on an API that requires basic authentication, you can use the second parameter to inform the user and password. It is necessary to inform an array containing the indexes `user` and` pass`.

```php
$client = new HttpClient(null, [
    'user' => 'username',
    'pass' => '123'
]);

$client->request("GET", "http://api.url.com");
$res = $client->toJson();

pre($res);
```

## Securing routes

### With Basic Auth

Basic authentication requires the username and password in the class constructor. If an index other than `user` and` pass` is informed, an exception will be thrown.

You can protect your routes through basic authentication as shown below.

```php
use Solital\Http\Auth\HttpAuth;

$auth = new HttpAuth([
    'user' => 'username',
    'pass' => '123'
]);
$auth->basic();
```

### With Digest Auth

In digest authentication, it is not necessary to inform anything in the constructor, just pass the allowed users with their respective passwords as a parameter in the `digest` method.

```php
use Solital\Http\Auth\HttpAuth;

$auth = new HttpAuth();
# 'username' => 'password'
$auth->digest(['admin' => 'pass1', 'admin2' => 'pass2', ...]);
```