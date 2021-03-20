# HTTP Cache

Font by [heroku]("https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers")

The modern day developer has a wide variety of techniques and technologies available to improve application performance and end-user experience. One of the most frequently overlooked technologies is that of the HTTP cache.

HTTP caching is a universally adopted specification across all modern web browsers, making its implementation in web applications simple. Appropriate use of these standards can benefit your application greatly, improving response times and reducing server load. However, incorrect caching can cause users to see out-of-date content and hard to debug issues. This article discusses the specifics of HTTP caching and in what scenarios to employ an HTTP cache header based strategy.

```php
use Solital\Core\Http\HttpCache;

$cache = new HttpCache();
```

## Cache-Control

The Cache-Control header is the most important header to set as it effectively ‘switches on’ caching in the browser. With this header in place, and set with a value that enables caching, the browser will cache the file for as long as specified. Without this header the browser will re-request the file on each subsequent request.

`public` resources can be cached not only by the end-user’s browser but also by any intermediate proxies that may be serving many other users as well.

`private` resources are bypassed by intermediate proxies and can only be cached by the end-client.

The value of the Cache-Control header is a composite one, indicating whether the resource is public or private while also indicating the maximum amount of time it can be cached before considered stale. The max-age value sets a timespan for how long to cache the resource (in seconds).

In the first parameter it is necessary to inform `public` or` private`, and in the second parameter the timestamp value of `max_age`. 

```php
$cache->cacheControl("public", 31536000);
```

While the Cache-Control header turns on client-side caching and sets the max-age of a resource the Expires header is used to specify a specific point in time the resource is no longer valid.

## Expires

When accompanying the Cache-Control header, Expires simply sets a date from which the cached resource should no longer be considered valid. From this date forward the browser will request a fresh copy of the resource. Until then, the browsers local cached copy will be used:

```php
$cache->cacheControl("public", 31536000);
$cache->expires();
```

## Time-based
A time-based conditional request ensures that only if the requested resource has changed since the browser’s copy was cached will the contents be transferred. If the cached copy is the most up-to-date then the server returns the `304` response code.

To enable conditional requests the application specifies the last modified time of a resource via the Last-Modified response header.

To use this condition, use the `lastModified` method after the `cacheControl` method. 

```php
$cache->cacheControl("public", 31536000);
$cache->lastModified();
```

The next time the browser requests this resource it will only ask for the contents of the resource if they’re unchanged since this date using the If-Modified-Since request header

```php
$cache->ifModifiedSince();
```

If the resource hasn’t changed, the server will return with an empty body with the `304` response code.

## Etag

The ETag (or Entity Tag) works in a similar way to the Last-Modified header except its value is a digest of the resources contents (for instance, an MD5 hash). This allows the server to identify if the cached contents of the resource are different to the most recent version.

Use the `eTag` class parameter the value that will be cached.

```php
$cache->cacheControl("public", 31536000);
$cache->eTag('value');
```

## Cache prevention
Highly secure or variable resources often require no caching. For instance, anything involving a shopping cart checkout process. Unfortunately, merely omitting cache headers will not work as many modern web browsers cache items based on their own internal algorithms. In such cases it is necessary to tell the browser to explicitly to not cache items.

In addition to public and private the Cache-Control header can specify no-cache and no-store which informs the browser to not cache the resources under any circumstances.

```php
$cache->noCacheControl();
```