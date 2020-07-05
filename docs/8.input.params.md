# Input & parameters

Solital offers libraries and helpers that makes it easy to manage and manipulate input-parameters like `$_POST`, `$_GET` and `$_FILE`.

## Using the Input class to manage parameters

You can use the `InputHandler` class to easily access and manage parameters from your request. The `InputHandler` class offers extended features such as copying/moving uploaded files directly on the object, getting file-extension, mime-type etc.

### Get single parameter value

```input($index, $defaultValue, ...$methods);```

To quickly get a value from a parameter, you can use the `input` helper function.

This will automatically trim the value and ensure that it's not empty. If it's empty the `$defaultValue` will be returned instead.

**Note:** 
This function returns a `string` unless the parameters are grouped together, in that case it will return an `array` of values.

**Example:**

This example matches both POST and GET request-methods and if name is empty the default-value "Guest" will be returned. 

```php
$name = input('name', 'Guest', 'post', 'get');
```

### Get parameter object

When dealing with file-uploads it can be useful to retrieve the raw parameter object.

**Search for object with default-value across multiple or specific request-methods:**

The example below will return an `InputItem` object if the parameter was found or return the `$defaultValue`. If parameters are grouped, it will return an array of `InputItem` objects.

```php
$object = input()->find($index, $defaultValue = null, ...$methods);
```

**Getting specific `$_GET` parameter as `InputItem` object:**

The example below will return an `InputItem` object if the parameter was found or return the `$defaultValue`. If parameters are grouped, it will return an array of `InputItem` objects.

```php
$object = input()->get($index, $defaultValue = null);
```

**Getting specific `$_POST` parameter as `InputItem` object:**

The example below will return an `InputItem` object if the parameter was found or return the `$defaultValue`. If parameters are grouped, it will return an array of `InputItem` objects.

```php
$object = input()->post($index, $defaultValue = null);
```

**Getting specific `$_FILE` parameter as `InputFile` object:**

The example below will return an `InputFile` object if the parameter was found or return the `$defaultValue`. If parameters are grouped, it will return an array of `InputFile` objects.

```php
$object = input()->file($index, $defaultValue = null);
```

### Managing files

```php
/**
 * Loop through a collection of files uploaded from a form on the page like this
 * <input type="file" name="images[]" />
 */

/* @var $image \Solital\Core\Http\Input\InputFile */
foreach(input()->file('images', []) as $image)
{
    if($image->getMime() === 'image/jpeg') 
    {
        $destinationFilname = sprintf('%s.%s', uniqid(), $image->getExtension());
        $image->move(sprintf('/uploads/%s', $destinationFilename));
    }
}

```

### Get all parameters

```php
# Get all
$values = input()->all();

# Only match specific keys
$values = input()->all([
    'company_name',
    'user_id'
]);
```

All object implements the `InputItemInterface` interface and will always contain these methods:

- `getIndex()` - returns the index/key of the input.
- `getName()` - returns a human friendly name for the input (company_name will be Company Name etc).
- `getValue()` - returns the value of the input.

`InputFile` has the same methods as above along with some other file-specific methods like:

- `getFilename` - get the filename.
- `getTmpName()` - get file temporary name.
- `getSize()` - get file size.
- `move($destination)` - move file to destination.
- `getContents()` - get file content.
- `getType()` - get mime-type for file.
- `getError()` - get file upload error.
- `hasError()` - returns `bool` if an error occurred while uploading (if `getError` is not 0).
- `toArray()` - returns raw array

---