# Cache (PSR-16)

Cache is a layer of high-speed physical data storage that holds a subset of data, usually temporary in nature, so that future requests for that data are answered more quickly than is possible when accessing the primary storage location of data. Caching allows you to efficiently reuse previously recovered or computed data.

## How to use

### Single cache

You can cache through PSR-16. To do this, perform the instance of the Cache class as follows:

```php
use Solital\Cache\Cache;

$cache = new Cache();

$list = $this->instance()->select()->build("all");

// The 'has' method checks whether the index exists
if ($cache->has('list') == true) {
    echo '<h1>from cache</h1>';
    // The 'get' method returns the cached value if it exists
    $cache->get('list');
} else {
    echo '<h1>created cache</h1>';
    // The 'set' method creates the cached file
    $cache->set('list', $list, 20);
}

// Displays the original content of the $list variable
echo '<h1>from original</h1>';
var_dump($list);
```

To delete the cache that was created, use the `delete` method passing the cache key as a parameter.

```php
$cache->delete('list');
```

The `has` method checks whether the item key exists. If it exists, use the `get` method to retrieve the generated cache by passing the key value as a parameter. If it does not exist, the `set` method creates the cached file, passing in the first parameter the name of the key, the value that will be stored and the time (in `int`) that this cached file will be valid.

### Mutiple cache

The syntax is similar to the single cache. But the `getMultiple` method needs an array containing the key values as a parameter. The `setMultiple` method generates the cache if it does not exist, but pass as an parameter an array in which the keys will be the indexes of the array, and in the last parameter spend the time that the cache will be valid.

```php
use Solital\Cache\Cache;

$cache = new Cache();

$list = [
    'nome' => 'Harvey Specter',
    'email' => 'specter@pearsonhardman.com'
];

$list2 = [
    'nome' => 'Louis Litt',
    'email' => 'liitup@pearsonhardman.com'
];

$cache->getMultiple(['list1', 'list2']);

echo '<h1>created cache</h1>';
$cache->setMultiple([
    'list1' => $list,
    'list2' => $list2
], 20);

echo '<h1>from original</h1>';
print_r($list);
print_r($list2);
```

In the multiple cache, use `deleteMultiple` by passing an array containing the cache keys to delete the cached files generated with the` setMultiple` method.

```php
$cache->deleteMultiple(["list1", "list2"]);
```

### Clear cache

To clear the entire cache created with the `Cache` class, use the`clear` function.

```php
$cache->clear();
```

## Wolf Template

To cache a template in Wolf, use the static cache function to set the cache file time as shown below:

```php
Wolf::cache(date('Hi'));
        
Wolf::loadView('home');
```