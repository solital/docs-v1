
# Installation

### Setting up Nginx

If you are using Nginx please make sure that url-rewriting is enabled.

You can easily enable url-rewriting by adding the following configuration for the Nginx configuration-file for the demo-project.

```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```

### Setting up Apache

Nothing special is required for Apache to work. We've include the `.htaccess` file in the `public` folder. If rewriting is not working for you, please check that the `mod_rewrite` module (htaccess support) is enabled in the Apache configuration.

Below is an example of an working `.htaccess` file used by Solital.

Simply create a new `.htaccess` file in your projects `public` directory and paste the contents below in your newly created file. This will redirect all requests to your `index.php` file (see Configuration section below).

```
RewriteEngine on
RewriteCond %{SCRIPT_FILENAME} !-f
RewriteCond %{SCRIPT_FILENAME} !-d
RewriteCond %{SCRIPT_FILENAME} !-l
RewriteRule ^(.*)$ index.php/$1
```

### Setting up IIS

On IIS you have to add some lines your `web.config` file in the `public` folder or create a new one. If rewriting is not working for you, please check that your IIS version have included the `url rewrite` module or download and install them from Microsoft web site.

#### web.config example

Below is an example of an working `web.config` file used by Solital.

Simply create a new `web.config` file in your projects `public` directory and paste the contents below in your newly created file. This will redirect all requests to your `index.php` file (see Configuration section below). If the `web.config` file already exists, add the `<rewrite>` section inside the `<system.webServer>` branch.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
	<rewrite>
	  <rules>
		<!-- Remove slash '/' from the en of the url -->
		<rule name="RewriteRequestsToPublic">
		  <match url="^(.*)$" />
		  <conditions logicalGrouping="MatchAll" trackAllCaptures="false">
		  </conditions>
		  <action type="Rewrite" url="/{R:0}" />
		</rule>

		<!-- When requested file or folder don't exists, will request again through index.php -->
		<rule name="Imported Rule 1" stopProcessing="true">
		  <match url="^(.*)$" ignoreCase="true" />
		  <conditions logicalGrouping="MatchAll">
			<add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
			<add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
		  </conditions>
		  <action type="Rewrite" url="/index.php/{R:1}" appendQueryString="true" />
		</rule>
	  </rules>
	</rewrite>
    </system.webServer>
</configuration>
```

#### Troubleshooting

If you do not have a `favicon.ico` file in your project, you can get a `NotFoundHttpException` (404 - not found).

To add `favicon.ico` to the IIS ignore-list, add the following line to the `<conditions>` group:

```
<add input="{REQUEST_FILENAME}" negate="true" pattern="favicon.ico" ignoreCase="true" />
```

You can also make one exception for files with some extensions:
```
<add input="{REQUEST_FILENAME}" pattern="\.ico|\.png|\.css|\.jpg" negate="true" ignoreCase="true" />
```

If you are using `$_SERVER['ORIG_PATH_INFO']`, you will get `\index.php\` as part of the returned value. 

**Example:**
```
/index.php/test/mypage.php
```

### Helper functions

Below is a list of all auxiliary functions present in Solital.

- `url()`: Handles the URI class
- `response()`: Handles the Response class
- `request()`: Handles the Request class
- `input()`: Get input class
- `uploadFile()`: Uploads a file
- `serverRequest()`: Handles the ServerRequest class
- `redirect()`: Redirect to another route
- `csrf_token()`: Get current csrf-token
- `pre()`: formatted var_dump