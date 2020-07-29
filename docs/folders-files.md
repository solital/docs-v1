# Handling Folders and Files

You can manipulate folders and files within Solital through the `HandleFiles` class, such as creating folders, removing folders, listing files and deleting files.

```php
use Solital\Core\Resource\HandleFiles;

$folder = new HandleFiles();
```

## List files within a folder

**list multiple files**

Use the `folder()` method to define the folder containing the files in Solital to be listed. To list all files within that folder, chain the `files()` method.

```php
/** Array return */
$res = $folder->folder("folder_name")->files();

pre($res);
```

**list single file**

```php
/** String return */
$res = $folder->folder("folder_name")->file('README.md');

pre($res);
```

To list only a single file within the folder, use the `file()` method passing as a parameter the file you want to search for

## Check if a file exists

To check if there is a file inside the folder, use `fileExists()`.

```php
/** Boolean return */
$res = $folder->folder("folder_name")->fileExists("README.md");

pre($res);
```

You can delete the file if it exists, to do so enter `true` in the second parameter.

```php
/** Boolean return */
$res = $folder->folder("folder_name")->fileExists("README.md", true);

pre($res);
```

## Create folder

To create a folder inside Solital, use only the `create()` method.

```php
/** Boolean return */
$res = $folder->create("folder_name");

pre($res);
```

You can define the type of permission the folder will have. The default is 0777.

```php
/** Boolean return */
$res = $folder->create("folder_name", 0755);

pre($res);
```

## Remove folder

To delete a folder inside Solital, use only the `remove()` method. This method will delete a folder if it is empty.

```php
/** Boolean return */
$res = $folder->remove("folder_name");

pre($res);
```

The `remove()` method checks for files inside the folder. If you want to delete the files inside the folder, pass `false` in the second parameter.

```php
/** Boolean return */
$res = $folder->remove("folder_name", false);

pre($res);
```