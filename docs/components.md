# Components

## Wolf Template 
Wolf is Solital's standard template system. You can load any template into the `resource/views` folder

### Basic
Below is the basic code to load any template:

```php
use Solital\Core\Wolf\Wolf;

Wolf::loadView('home');
```
        
### Parameters
The sitaxe below loads the parameters to be viewed in your template.

```php
Wolf::loadView('home', [
    'title' => 'My Title'
]);
```
        
And in your `home.php`, retrieve the value informed in this way:

```html
<title>$title</title>
```
        
### Custom extensions
Wolf will search for files in php format, but to search for a different format, use the last parameter.

```php
Wolf::loadView('home', [
    'title' => 'My Title'
], false, "html");
```
        
### Loading CSS, JS and images
Make sure the files exist in the folder `public/assets/_css`, `public/assets/_js` and `public/assets/_img`

To load a CSS file, use the static `loadCss` method in your template.

```html
<link rel="stylesheet" href="<? self::loadCss('style.css'); ?>">
```
        
To load a JS file, use the static `loadJs` method in your template.

```html
<link rel="stylesheet" href="<? self::loadJs('file.js'); ?>">
```
        
To load a image file, use the static `loadImg` method in your template.

```html
<img src="<? self::loadImg('image.png'); ?>">
```

To load a file outside the `_css`,` _js` and `_img` folder, use the` loadFile` method.

```html
<img src="<? self::loadFile('path/for/your/file'); ?>">
```
        
### Cache
See the cache part [here](https://solital.github.io/docs-v1/cache/) to learn how to use the cache in Wolf.

## Message

Message helps you when displaying messages in your view. Its syntax is basic as shown below.

To create a new message:

```php
use Solital\Core\Resource\Message;

Message::new("your_index_message", "your_messsage");
```

To retrieve a message:

```php
use Solital\Core\Resource\Message;

Message::get("your_index_message");
```

To delete a message:

```php
use Solital\Core\Resource\Message;

Message::clear("your_index_message");
```

To recover and then delete a message:

```php
use Solital\Core\Resource\Message;

Message::get("your_index_message");
Message::clear("your_index_message");
```

## Mail

Mail is a class of Solital that uses PHP's native mail to send email.

### Use

The sitaxis below is used to be able to send basic e-mail.

```php
use Solital\Core\Resource\Mail;

Mail::send("your_sender@email.com", "your_recipient@email.com", 
"your_subject", "your_message");
```

### Validate

To check if an email is really valid, use the `validateEmail` function.

```php
$res = Mail::validateEmail("brenno.gnr@gmail.com");

pre($res);
```
        
### Optional parameters

To add a reply, text type, charset and priority, use the optional parameters.

```php
Mail::send("your_sender@email.com", "your_recipient@email.com", "your_subject", 
"your_message", "your_reply@email.com", "type_text", "your_charset", your_priority);

```

Optional parameters have the following values by default:

- Reply to: `(string)` null
- Type: `(string)` text/plan
- Charset: `(string)` UTF-8
- Priority: `(int)` 3