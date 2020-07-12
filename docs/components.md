# Components

## Wolf Template 
Wolf is Solital's standard template system. You can load any template into the `resource/views` folder

### Basic
Below is the basic code to load any template:

```php
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

To upload a CSS file, use the syntax below in your template

```html
<link rel="stylesheet" href="<? self::loadCss('style.css'); ?>">
```
        
To upload a JS file, use the syntax below in your template

```html
<link rel="stylesheet" href="<? self::loadJs('file.js'); ?>">
```
        
To upload an image, use the syntax below in your template

```html
<img src="<? self::loadImg('image.png'); ?>">
```
        
### Cache
See the cache part [here](https://solital.github.io/docs-v1/13.cache/) to learn how to use the cache in Wolf.

## Message

Message helps you when displaying messages in your view. Its syntax is basic as shown below.

To create a new message:

```php
use Solital\Message\Message;

Message::newMessage("your_index_message", "your_messsage");
```

To retrieve a message:

```php
use Solital\Message\Message;

Message::getMessage("your_index_message");
```

To delete a message:

```php
use Solital\Message\Message;

Message::clearMessage("your_index_message");
```

To recover and then delete a message:

```php
use Solital\Message\Message;

Message::getMessage("your_index_message");
Message::clearMessage("your_index_message");
```

## Mail

Mail is a class of Solital that uses PHP's native mail to send email.

### Use

The sitaxis below is used to be able to send basic e-mail.

```php
use Solital\Mail\Mail;

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
use Solital\Mail\Mail;

Mail::send("your_sender@email.com", "your_recipient@email.com", "your_subject", 
"your_message", "your_reply@email.com", "type_text", "your_charset", your_priority);

```

Optional parameters have the following values by default:

- Reply to: `(string)` null
- Type: `(string)` text/plan
- Charset: `(string)` UTF-8
- Priority: `(int)` 3