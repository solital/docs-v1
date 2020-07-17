# Getting Started

Katrina ORM is a component to bring the object-oriented application development paradigm closer to the relational database paradigm. It helps when carrying out common routines, such as the famous CRUD (create, read, edit and delete), in addition to having a login and data paging system.

### Requirements

Make sure that the PDO is enabled in your development environment or in your hosting.

### Installation

Katrina ORM is already installed by default in Solital. But if you are going to install in another project, use the command below to download via Composer.

```
composer require solital/katrina
```

Or add the code below to your `composer.json` file.

```
"require": {
    "solital/katrina": "1.0.*"
}
```

### Settings

If you are using the Solital framework, run the `php vinci katrina:configure` command to configure the `db.php` file via Vinci ([see more](https://solital.github.io/docs-v1/vinci/#configure-database) about configuring Katrina ORM No Vinci here)

Or edit (in Solital) the `db.php` file inside the `config` folder.

```php
define('DB_CONFIG', [
    'DRIVE' => 'your_drive',
    'HOST' => 'your_host',
    'DBNAME' => 'your_database_name',
    'USER' => 'your_user',
    'PASS' => 'your_password'
]);
```

## Initial structure

You can use katrina in two ways:

1°) In Solital, extend the model already created and define the variables `$table`, `$primaryKey` and `$columns` in your model's constructor as listed below:

```php
<?php

namespace Solital\Components\Model;
use Solital\Components\Model\Model;

class User extends Model
{
    public function __construct()
    {
        $this->table = 'your_database_table';
        $this->primaryKey = 'primary_key_of_the_table';
        $this->columns = [
            'first_column_of_the_table', 
            'second column of the table',
            #...
        ];
    }

    public function get()
    {
        return $this->instance()->select()->build("ALL");
    }
}
```

2°) Or if you are using it in another project

```php
<?php

use Katrina\Katrina as Katrina;

class User
{

    # String
    private $table = 'your_database_table';
    # String
    private $primaryKey = 'primary_key_of_the_table';
    # Array
    private $columns = [
        'first_column_of_the_table', 
        'second column of the table',
        #...
    ];

    public function instance()
    {
        $katrina = new Katrina($this->table, $this->columnPrimaryKey, $this->columns);
        return $katrina;
    }

    public function get()
    {
        return $this->instance()->select()->build("ALL");
    }
}
```

## Data manipulation

### List

To list all fields in the table, use `select()` as shown in the previous example. By default, the method will list all fields in the table.

```php
public function get()
{
    return $this->instance()->select()->build("ALL");
}
```

To list a single value, pass the table field `id` as a parameter, and in `build()` method use ONLY.

```php
public function get()
{
    return $this->instance()->select(3)->build("ONLY");
}
```

To specify which fields you want to list, pass the values ​​as parameters.

```php
public function get()
{
    return $this->instance()->select("name, city, country")->build();
}
```

If you need the `WHERE` clause, use the second parameter.

### Listing foreign key

**Inner join**

The `innerJoin()` method returns the values of two tables that have a foreign key. Enter the name of the table and the field of the other table that has the primary key for your main table.

```php
public function get()
{
    $res = $this->instance()->innerJoin("address", "idAddress")->build("ALL");
    return $res;
}
```

If you want to return only a single value, use the third parameter passing the primary key.

```php
public function get()
{
    $res = $this->instance()->innerJoin("address", "idAddress", 2)->build("ALL");
    return $res;
}
```

You can inform which fields you want to return. "a" is your main table while "b" is your table that has the foreign key.

```php
public function get()
{
    $res = $this->instance()
    ->innerJoin("address", "idAddress", 2 "a.idPerson, a.name, b.street", "address", "idAddress")->build("ALL");
    return $res;
}
```


### Insert
 
The `insert()` method inserts the values ​​into the table. It is NOT necessary to use `build()` method to insert the data. To do this, create an array with the values ​​that the method will receive

```php
public function insert()
{
    $res = $this->instance()->insert(['Clark', 'Metropolis', 'EUA']);
    return $res;
}
```

### Update

The `update()` method updates the values ​​in the table. It is NOT necessary to use `build()` method to update the data. The process is similar to the insert method. The first parameter is the columns that will be updated, the second parameter the values ​​and the third the row `id`.

```php
public function update()
{
    $res = $this->instance()->update(['name', 'age'], ['Specter', '41'], 8);
    return $res;
}
```

### Delete

The `delete()` method deletes the values ​​in the table. Enter the primary key corresponding to the table row you want to delete.

```php
public function delete()
{
    $res = $this->instance()->delete(3)->build();
    return $res;
}
```

## Manipulating tables

### Create a new table

The `createTable()` method starts opening the table. After inserting the fields and data types that the tables will have, use `closeTable()` to close the table. For a better understanding see the syntax below.

```php
$res = $this->instance()
            /* Starts the table by specifying its name */
            ->createTable("your_table_name")
            /* Fields and table type */
            ->int("id_orm")->primary()->increment()
            ->varchar("name", 20)->unique()->notNull()->default("specter")
            ->int("age", 3)->unsigned()->notNull()
            ->varchar("email", 30)->default("harvey.specter@gmail.com")->notNull()
            ->varchar("profession", 40)
            ->int("tipo")
            ->constraint("dev_cons_fk")->foreign("type")->references("dev", "iddev")
            /* Close the command to create the table */
            ->closeTable()
            /* Compile the code above */
            ->build();
```

### List tables

To have a list of all the tables in your database, use the `listTables()` method by passing `ALL` in the `build()` method.

```php
public function get()
{
    $res = $this->instance()->listTables()->build("ALL");
    return $res;
}
```

### List columns

To list the columns of a table, use the `describeTable()` method passing as a parameter the name of your table together with `ALL` in the `build()`

```php
public function get()
{
    $res = $this->instance()->describeTable("your_table")->build("ALL");
    return $res;
}
```

### Alter table

The `alter()` method performs the procedures of adding, changing and deleting a field from the database table.

**Add new field**

Use `add()` method together with the data type to add a new field.

```php
public function get()
{
    $res = $this->instance()
                ->alter("message")->add()
                ->varchar("first_field", 10)
                ->build();
}
```

**Drop column**

Use the `drop()` method to delete a column from the table.

```php
public function get()
{
    $res = $this->instance()
                ->alter("message")->drop("type")
                ->build();
}
```

**Modify column**

Use the modify SQL with the `modify()` method.

```php
public function get()
{
    $res = $this->instance()
                ->alter("message")->modify()
                ->varchar("person_type", 100)
                ->build();
}
```

**Change column**

Use the `change()` method to change a column. As a parameter, pass the current column name.

```php
public function get()
{
    $res = $this->instance()
                ->alter("message")->change("person_type")
                ->varchar("type", 100)
                ->build();
}
```

### Rename table

Use the `rename()` method to rename a database table. Use the first parameter the current table name and the second parameter the new table name.

```php
public function get()
{
    $res = $this->instance()
                ->rename("message", "new_message")
                ->build();
}
```

### Adding foreign key

To add a foreign key to an already created table, use the `addConstraint()` method to add a constraint; `foreign()` to inform the column and `references()` to refer to the table.

```php
public function get()
{
    $res = $this->instance()
                ->alter("message")->addConstraint("dev_cons_fk")->foreign("type")->references("dev", "iddev")
                ->build();
}
```

NOTE: if you are creating a new table, use the `constraint()` method instead of `addConstraint()` as shown below:

```php
#...
->constraint("dev_cons_fk")->foreign("type")->references("dev", "iddev")
#...
```

### Drop table

To delete a table from the database, use the `dropTable()` method.

```php
public function get()
{
    $res = $this->instance()
                ->dropTable("message")
                ->build();
}
```

### Truncate table

To use the sql truncate command, use the `truncate()` method.

```php
public function get()
{
    $res = $this->instance()
                ->truncate()
                ->build();
}
```

By default, the database checks the table's foreign key and locks the truncate command. To disable foreign key verification, enter `true` as a parameter.

```php
public function get()
{
    $res = $this->instance()
                ->truncate(true)
                ->build();
}
```

## Procedure

To call a database procedure, use the `call()` method.

```php
public function get()
{
    $res = $this->instance()->call('procedure_name');
    return $res;
}
```

To use procedure parameters, pass the values in array format.

```php
public function get()
{
    $res = $this->instance()->call('procedure_name' , ['param_1, param_2, param_3']);
    return $res;
}
```

## Pagination

The `pagination()` method creates a system for paging results. To initialize, the first parameter must be the table you want to use to start paging. The second parameter will list the amount of values that will be returned from the table as shown in the example below.

```php
public function get()
{
    $res = $this->instance()->pagination('your_table', 3);
    return $res;
}
```

The above method will return an array containing `rows` indexes that will return values, and `arrows` that will return commands for pagination. To use in the Wolf template, use it this way.

```php
$html = $this->instance()->pagination('your_table', 3);

Wolf::loadView('home', [
    'rows' => $html['rows'],
    'arrows' => $html['arrows']
]);
```

And in your view, return the results that way.

```html
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
            <th>Gender</th>
        </tr>
    </thead>

    <tbody>
        <?php foreach ($rows as $r): ?>
            <tr>
                <td><?= $r['name'] ?></td>
                <td><?= $r['age'] ?></td>
                <td><?= $r['gender'] ?></td>
            </tr>
        <?php endforeach; ?>
    </tbody>
</table>

<?php
echo $arrows;
```

The result will be as follows:


| Name  | Age | Gender |
|-------|-----|--------|
| Sam   | 47  | Male   |
| Dean  | 49  | Male   |
| Marry | 52  | Female |

```html
<< 1 2 3 >>
```

To change the arrows (`<<` and `>>`), use the last two parameters of the `pagination()` method.

```php
public function get()
{
    $res = $this->dbInstance()->pagination('tb_users', 3, 'First Item', 'Last Item');
    return $res;
}
```

The result will be:

```html
First item 1 2 3 Last item
```

## Types of data

Below is listed the attributes and data supported by Katrina ORM:

**String data**

| Types                             |
|-----------------------------------|
| `varchar("column_name", size)`    |
| `char("column_name", size)`       |
| `tinytext("column_name", size)`   |
| `mediumtext("column_name", size)` |
| `longtext("column_name", size)`   |
| `text("column_name", size)`       |

**Numerical data**

| Types                                    |
|------------------------------------------|
| `tinyint("column_name", size)`           |
| `smallint("column_name", size)`          |
| `mediumint("column_name", size)`         |
| `bigint("column_name", size)`            |
| `int("column_name", size)`               |
| `decimal("column_name", value1, value2)` |

**Date and time**

| Types                       |
|-----------------------------|
| `date("column_name")`       |
| `year("column_name")`       |
| `time("column_name")`       |
| `datetime("column_name")`   |
| `timestamp("column_name")`  |

**Boolean**

| Types                       |
|-----------------------------|
| `boolean("column_name")`    |

**Attributes**

| Types                      |
|----------------------------|
| `default("default_value")` |
| `unique()`                 |
| `unsigned()`               |
| `incremet()`               |
| `notNull()`                |
| `primary()`                |