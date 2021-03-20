# JSON

Starting with version 1.3 of Solital, it is possible to manipulate JSON using the `JSON` class. 
The difference of this class for the `json_encode` and` json_decode` functions, is 
that you can manipulate `.json` files and display errors when encode and decoding a JSON. 

```php
use Solital\Core\Resource\Json;

$json = new Json();
```

## Predefined Constants

By default, the `JSON_UNESCAPED_UNICODE` constant is defined in the constructor of the` JSON` class. It is possible to add more than one constant following the model below: 

```php
use Solital\Core\Resource\Json;

$json = new Json(JSON_HEX_TAG | JSON_HEX_AMP | JSON_UNESCAPED_UNICODE);
```

## Encode JSON

The `encode` function creates a JSON from an array. 

```php
$array = ["name" => "Adithýa", "age" => 20];

$json = new Json();
$res = $json->encode($array);

/* Return JSON */
pre($res);
```

## Decode JSON

```php
$json_file = '{"Organization": "PHP Documentation Team"}';

$json = new Json();
$res = $jsons->decode($json_file);

/* Return object */
pre($res);
```

The `decode` method returns an object. To return an associative array, use `true` in the second parameter.

```php
$json_file = '{"Organization": "PHP Documentation Team"}';

$json = new Json();
$res = $jsons->decode($json_file, true);

/* Return array */
pre($res);
```

## Returning a value in JSON

If you need to read a value from the JSON file, use the `inJson` method. Inform JSON in the first parameter and the name of the key that contains the value in the second parameter. 

```php
$json_file = '{"Organization": "PHP Documentation Team"}';

$json = new Json();
$res = $jsons->inJson($json , 'Organization');

/* Return string */
pre($res);
```

## Read an external JSON file

If you want to read an external file, use `readFile`. This method works in a similar way to the `decode` method. 

```php
$json_file = '{"Organization": "PHP Documentation Team"}';

$json = new Json();
$res = $jsons->readFile('data.json');

/* Return object */
pre($res);
```

**Returning in array**

```php
$json_file = '{"Organization": "PHP Documentation Team"}';

$json = new Json();
$res = $jsons->readFile('data.json', true);

/* Return array */
pre($res);
```

## Returning errors

A JSON containing the type of error is returned whenever there is a failure to code or 
decode a JSON. Below is an example of how it is returned: 

```json
{
    "json_error": "Syntax error"
}
```