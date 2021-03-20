# Validation

You can validate email, string, array and others.

To start, make an instance of the `Validation` class: 

```php
use Solital\Core\Resource\Validation;

$valid = new Validation();
```

## Validate email, string and more

### Email

```php
$valid = new Validation();
$res = $valid->email('solital@email.com');

/* Return `string` if true or `null` if false */
pre($res);
```

### Number

You can validate whether a number is `int` or` float`. 

```php
$valid = new Validation();
$res = $valid->number(12.5);

/* Return `int` or `float` if true or `null` if false */
pre($res);
```

### Null

```php
$valid = new Validation();
$res = $valid->isNull(null);

/* Return bool */
pre($res);
```

### Lowercase

You can validate a string if it is lowercase. If not, the `isLower` method will 
convert the string to lowercase. 

```php
$valid = new Validation();
$res = $valid->isLower('SOLITAL');

/* Return string */
pre($res);
```

### Uppercase

You can validate a string if it is uppercase. If not, the `isUpper` method will 
convert the string to uppercase. 

```php
$valid = new Validation();
$res = $valid->isUpper('solital');

/* Return string */
pre($res);
```

## Validate Date

It is possible to validate dates and times using the `Convertime` class. 
To start, perform the instance: 

```php
use Solital\Core\Resource\Convertime;

$convertime = new Convertime();
```

In the class constructor, you can define the timezone. By default, the default 
timezone is set to `America/Sao_Paulo`.

```php
$convertime = new Convertime("America/Fortaleza");
```

### Format date

To format a date, enter the date you want to convert and the format to be converted.

```php
$convertime = new Convertime();
$res = $convertime->formatDate('04/01/1999', 'Y-m-d');

/* Return 1999-01-04 */
pre($res);
```

### Add months to a date

In some cases, you may need to add months to a specific date. To do this, use the `addMonth` class. 
This class is similar to the `formatDate` class, with the difference that you must enter 
in the last parameter the number of months that will be added to the date.

This class already has conversion for days with 28, 29, 30 or 31 days. 

```php
$convertime = new Convertime();
$res = $convertime->addMonth('1999-01-04', 'd/m/Y', 1);

/* Return 1999-02-04 */
pre($res);
```

### Add days to a date

To add days to a date, the `addDays` method works in a similar way to the` addMonth` method.  

```php
$convertime = new Convertime();
$res = $convertime->addDays('1999-01-04', 'd/m/Y', 3);

/* Return 1999-01-07 */
pre($res);
```

### Add time to another time

It is possible to add a specific time to another time. For example: add 3 more hours at 13:00.

```php
$convertime = new Convertime();
$res = $convertime->addHour('13:00', '03:00');

/* Return 16:00 */
pre($res);
```