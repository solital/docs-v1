# Getting Started

Katrina ORM is a component to bring the object-oriented application development paradigm closer to the relational database paradigm. It helps when carrying out common routines, such as the famous CRUD (create, read, edit and delete), in addition to having a login and data paging system.

### Requirements

- PHP >= 7.2 (Compatible with PHP 8)
- PHP PDO extension enabled

### Installation

Katrina ORM is already installed by default in Solital. But if you are going to install in another project, use the command below to download via Composer.

```
composer require solital/katrina
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

### For SQLite

Some differences exist for connecting to an SQLite database. First, add an additional index called `SQLITE_DIR` in the `DB_CONFIG` constant. This constant must have the absolute path where the SQLite database file will be located.

Then, assign the value `sqlite` in the index `DRIVE`.

```php
define('DB_CONFIG', [
    'DRIVE' => 'sqlite',
    'DBNAME' => 'your_database_name.db',
    'SQLITE_DIR' => '/path/to/file/'
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

## Data manipulation - CRUD

### List

To list all fields in the table, use `select()` as shown in the previous example. By default, the method will list all fields in the table.

```php
public function get()
{
    return $this->instance()->select()->build("ALL");
}
```

To list a single value, pass the table field `id` as a parameter, and in `build()` method use `ONLY`.

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
    return $this->instance()->select(null, null, "name, city, country")->build("ALL");
}
```

If you need the `WHERE` clause, use the second parameter.

```php
public function get()
{
    return $this->instance()->select(null, 'name="Clark"', "name, city, country")
                            ->build("ALL");
}
```

With primary key:

```php
public function get()
{
    return $this->instance()->select(3, 'name="Clark"', "name, city, country")
                            ->build("ONLY");
}
```

### Listing foreign key

**Inner join**

The `innerJoin ()` method returns the values of two tables that have a foreign key.

The first parameter will be the name of the table that has a relationship with the current table. The second will be an array containing in the first index the column name of the current table that has the foreign key, and in the second index the column name of the primary key of the other table. To make it easier, see an example below.

```php
public function get()
{
    return $this->instance()->innerJoin("address", ["idForeignAddress", "idAddress"])
                            ->build("ALL");
}
```

If you need to use the `WHERE` clause, pass the command in the third parameter as shown below.

```php
public function get()
{
    return $this->instance()->innerJoin("address", ["idForeignAddress", "idAddress"], "order_status=true")->build("ALL");
}
```

You can inform which fields you want to return. "a" is your main table while "b" is your table that has the foreign key.

```php
public function get()
{
    return $this->instance()->innerJoin("address", ["idForeignAddress", "idAddress"], "order_status=true", "a.idPerson, a.name, b.street", "address", "idAddress")->build("ALL");
}
```

### Custom SELECT

You can create a custom SELECT statement. To do this, use the function `customQueryOnly` to return a single value from the database, and` customQueryAll` to return all values from the database.

```php
public function getAll()
{
    return $this->instance()->customQueryAll("SELECT a.idSession, SUM(b.price) AS price, 
    SUM(a.qtd) AS qtd FROM tb_order a INNER JOIN tb_product b 
    WHERE MONTH( a.created_at) = MONTH(NOW()) GROUP BY a.idSession");
}
```

```php
public function getOnly()
{
    return $this->instance()->customQueryOnly("SELECT a.idSession, SUM(b.price) AS price, 
    SUM(a.qtd) AS qtd FROM tb_order a INNER JOIN tb_product b 
    WHERE MONTH( a.created_at) = MONTH(NOW()) GROUP BY a.idSession");
}
```

### Insert
 
The `insert()` method inserts the values ​​into the table. It is NOT necessary to use `build()` method to insert the data. To do this, create an array with the values ​​that the method will receive

```php
/**
 * Return bool
 */
public function insert()
{
    $res = $this->instance()->insert(['Clark', 'Metropolis', 'EUA']);
    return $res;
}
```

To return the last insert ID, pass a `true` in the second parameter.

```php
/**
 * Return array
 * 
 * ['res'] => 'true',
 * ['lastId'] => '2'
 */
public function insert()
{
    $res = $this->instance()->insert(['Clark', 'Metropolis', 'EUA'], true);
    return $res;
}
```

### Update

The `update()` method updates the values ​​in the table. It is NOT necessary to use `build()` method to update the data. The process is similar to the insert method. The first parameter is the columns that will be updated, the second parameter the values ​​and the third the row `id`. You can use an integer or a string in the third parameter

```php
public function update()
{
    $res = $this->instance()->update(['name', 'age'], ['Specter', '41'], "id=3");
    return $res;
}
```

Or

```php
public function update()
{
    $res = $this->instance()->update(['name', 'age'], ['Specter', '41'], 3);
    return $res;
}
```

### Delete

The `delete()` method deletes the values ​​in the table. Enter the value of the line to be deleted, the value being the primary key.

```php
public function delete()
{
    $res = $this->instance()->delete(3)->build();
    return $res;
}
```

Or a string

```php
public function delete()
{
    $res = $this->instance()->delete("Bruce")->build();
    return $res;
}
```

By default, the `delete` method uses the column name of the primary key to delete the row, but you can use the name of another column using the second parameter.

```php
public function delete()
{
    $res = $this->instance()->delete("Bruce", "name")->build();
    return $res;
}
```

** Force delete with foreign key **

In some cases there may be a need to delete a record with the foreign key from another table. To disable foreign key checking, you can use the third parameter as `true`.

```php
public function delete()
{
    $res = $this->instance()->delete(3, null, true)->build();
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

**Rename table**

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

The above method will return an array containing `rows` indexes that will return values, and `arrows` that will return commands for pagination. 

To use pagination with relationship in another table, in the third parameter pass an array containing the name of the table that has a relationship with the current table, the column name of the current table that has the foreign key and the column name of the primary key of the another table.

```php
public function get()
{
    $res = $this->instance()->pagination('your_table', 3, ['foreign_table', 'column_foreign_key', 'column_primary_key']);
    return $res;
}
```

**INNER JOIN**

If you want to use pagination with a table that has a foreign key, pass an array in the third parameter.

In the first index, insert the name of the table that is linked to the current table, in the second index the name of the column that contains the foreign key and in the third index the column name of the primary key of the table that references the current table

```php
public function get()
{
    $res = $this->instance()->pagination('your_table', 3, ['foreign_table', 'column_foreign_key', 'column_primary_key'], "status=true");
    return $res;
}
```

**WHERE clause**

To use the WHERE clause, use the fourth parameter as shown below.

```php
public function get()
{
    $res = $this->instance()->pagination('your_table', 3, null, "status=true");
    return $res;
}
```

**Wolf Templte**

To use in the Wolf template, use it this way.

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

To change the arrows (`<<` and `>>`), use the last two parameters of the `pagination()` method. The result will be:

```php
public function get()
{
    $res = $this->instance()->pagination('your_table', 3, null, null, "First", "Last");
    return $res;
}
```

| Name  | Age | Gender |
|-------|-----|--------|
| Sam   | 47  | Male   |
| Dean  | 49  | Male   |
| Marry | 52  | Female |

```html
First 1 2 3 Last
```

## Custom Pagination

If you have a very complex SELECT statement, you can use the `customPagination` method. This method already has a `LIMIT` by default, in addition to being able to change the name of the arrows.

```php
public function get()
{
    $res = $this->instance()->customPagination("SELECT created_at, order_status, idSession, SUM(idOrder) AS idOrder FROM `tb_order` GROUP BY created_at, order_status, idSession", 3, "First", "Last");
    
    return $res;
}
```

**Customizing arrows CSS**

You can customize the look of the arrows through the classes `pagination_first_item`, `pagination_atual_item`, `pagination_others_itens` and `pagination_last_item`.

Below is a customization to serve as an example:

```css
.pagination_atual_item {
    background-color: #B5B5B5;
    padding: 10px;
    margin: 5px;
    border-radius: 5px;
    margin-top: 30px;
    transition: 0.2s;
}

.pagination_first_item, .pagination_others_itens, .pagination_last_item {
    background-color: #4682B4;
    color: #FFF;
    padding: 10px;
    margin: 5px;
    border-radius: 5px;
    margin-top: 30px;
    transition: 0.2s;
}

.pagination_first_item:hover, .pagination_others_itens:hover, .pagination_last_item:hover {
    background-color: #0071E3;
    color: #FFF !important;
    transition: 0.2s;
}
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
| `text("column_name")`             |

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
| `after("column_name")`     |
| `first()`                  |