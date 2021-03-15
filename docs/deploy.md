# Deploy

To deploy your project made in Solital Framework, do a `git pull` on your hosting.

Then, update all components using `composer update`. Solital may receive some important updates during this period of development.

## Apache

There is a `htaccess` file inside the `public/` folder, but Apache needs to point to this folder. Therefore, in the root folder of your project, insert this other `htaccess` file below:

```
RewriteEngine on

# Force HTTPS
RewriteCond %{HTTPS} off

# Redirect to public/ with HTTPS
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
RewriteCond %{HTTP_HOST} ^YOUR_DOMAIN.com$ [NC,OR]
RewriteCond %{HTTP_HOST} ^www.YOUR_DOMAIN.com$
RewriteCond %{REQUEST_URI} !public/
RewriteRule (.*) /public/$1 [L]
```

Replace `YOUR_DOMAIN` by the name of your primary domain. 