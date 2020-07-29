# Security

## Hash

To create an encrypted key, use the `Hash` class together with the static `encrypt` function as shown below:

```php
use Solital\Core\Security\Hash;

$res = Hash::encrypt('word_to_encrypt');

pre($res);
```

You can define how long this key will be valid. It can be 1 second, 1 hour or 1 year. by default the value is `+1 hour`.

```php
use Solital\Core\Security\Hash;

$res = Hash::encrypt('word_to_encrypt', '+1 month');

pre($res);
```

If you want to decrypt, use the `decrypt` function chained with the `value` method.

```php
use Solital\Core\Security\Hash;

$res = Hash::decrypt('word_to_decrypt')::value();

pre($res);
```

If you want to check if the encrypted key is still valid, use `isValid`. If you want to verify that the encrypted key is still valid, use `isValid`. the `isValid` method will return` true` if it is still valid, and `false` if it is already expired

```php
use Solital\Core\Security\Hash;

$res = Hash::decrypt('word_to_decrypt')::isValid();

pre($res);
```

## Forgot password

Solital has a standard method for password recovery. For that, it is necessary to configure only the constant `EMAIL` in the file `config.php`, inserting the sender and recipient.

The `Reset` class uses php's native `mail` method for sending e-mail.

```php
use Solital\Core\Security\Reset;

public function forgot()
{    
    $email = input()->post('email')->getValue();

    (new Reset())->table('your_database_table', 'your_column_table')
                 ->forgotPass($email, "/your_redirect_url", "+20 minute");

    response()->redirect('/home');
}
```

Instantiate the `Reset` class. In the `table` function, the first parameter should be the name of your table where users’ emails are stored, and in the second parameter the column name where emails are stored, and then chain with the `forgotPass` method.

In the `forgotPass` method, pass as first parameter the email you want to retrieve, and in the second the url in which the user will be redirected when clicking on the email link. The third parameter is optional, the time that the key will be valid will be defined. The default is `+1 hour`

To validate the information by clicking on the email link, you can use the structure below:

```php
public function change($hash)
{
    $res = Hash::decrypt($hash)::isValid();
    
    if ($res == true) {
        $email = Hash::decrypt($hash)::value();

        Wolf::loadView('auth.change', [
            'email' => $email,
            'hash' => $hash
        ]);
    } else {
        response()->redirect('/home');
    }
}
```

## CSRF Protection

Any forms posting to `POST`, `PUT` or `DELETE` routes should include the CSRF-token. We strongly recommend that you enable CSRF-verification on your site to maximize security.

You can use the `BaseCsrfVerifier` to enable CSRF-validation on all request. If you need to disable verification for specific urls, please refer to the "Custom CSRF-verifier" section below.

By default Solital will use the `CookieTokenProvider` class. This provider will store the security-token in a cookie on the clients machine.
If you want to store the token elsewhere, please refer to the "Creating custom Token Provider" section below.

### Adding CSRF-verifier

When you've created your CSRF-verifier you need to tell Solital that it should use it. You can do this by adding the following line in your `routes.php` file:

```php
Course::csrfVerifier(new \Solital\Core\Http\Middleware\BaseCsrfVerifier());
```

### Getting CSRF-token

When posting to any of the urls that has CSRF-verification enabled, you need post your CSRF-token or else the request will get rejected.

You can get the CSRF-token by calling the helper method:

```php
csrf_token();
```

You can also get the token directly:

```php
return Course::router()->getCsrfVerifier()->getTokenProvider()->setToken();
```

The default name/key for the input-field is `csrf_token` and is defined in the `POST_KEY` constant in the `BaseCsrfVerifier` class.
You can change the key by overwriting the constant in your own CSRF-verifier class.

**Example:**

The example below will post to the current url with a hidden field "`csrf_token`".

```html
<form method="post" action="<?= url(); ?>">
    <?= csrf_token(); ?>
    <!-- other input elements here -->
</form>
```
---