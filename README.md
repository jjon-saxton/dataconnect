# dataconnect
Extensions to PDO to make working with data in an SQL database easier and a bit more secure!

## Installation

In theory, installation is fairly easy and straightforward. Simply add the single file `database.inc.php` to your project and use php's `include` or `require` keywords to your main project file so you can use the classes contained within.

## Configuration

Before you can use the classes, however, you **must** configure the script to work with your database instance. At this time it only supports MySQL, but we will be working on supporting other databases in the future. An example `settings.ini` file is included for you in the root folder alongside this README. Comments are provided to help guide you through the processes and options, if applicable.

## Basic Usage

This script adds two classes which help you more easily manage data in a database while minimizing the risk of cross-site SQL intrusions. The two classes are `DataBaseSchema` which helps you manage things at the schema level, namely creating and listing tables, and `DataBaseTable` which help you manage data at the table level, getting/putting/updating rows for example.

### Main Methods (DataBaseSchema)

Below is a list of main methods and a *brief* description of what they do and how to use them. To use these methods be sure to create an object like so

```PHP
$dbschema=new DataBaseSchema($schema,$settingsfile)= //where $schema is the name of your database schema and $settingsfile is the path to your settings.ini. The default for $schema is pulled from appropriate value in your settings.ini and if that path is blank the script assumes it is in the same folder
```

* DataBaseSchema::addTable([name] array [cols]) - Takes a name and an array of columns and creates a new table with the keys of the array as column names, and values as attributes.
* DataBaseSchema::showTables() - returns an array of tables in the current schema.

### Main Methods (DataBaseTable)

At present, DataBaseTable extends DataBaseSchema so you do not need to pass that object to it. To initialize a table connection you only need to:

```PHP
$dbtable=new DataBaseTable($table,$short,$settingsfile,$schema) //The $schema and $settingsfile variables are exactly like those above. $table is the name of your table $short is a boolean value, true allows you to not specify the table prefix when applicable, default value is false which requires the full table name
```

* DataBaseTable::putData($data) - Puts an array of data into a table. Keys of the array would be column names and values the column values, be sure your data is formatted as your table and schema expect or this will report an error
* DataBaseTable::updateData($data) - Works *exactly* like the above only it requires one column to represent the primary key and value of an *existing* row so it can update that row rather than add a new one.
* DataBaseTable::getData($query) - retrieves data from a table. If $quuery is null than an array of all data in the table is returned, otherwise $query is a special string that will filter the data down. Formatting the special $query string will be discussed later.

## Building a Basic Query String

For `DataBaseTable::getData($query)` a specially formated string is used to filter database results. This is designed in such away that it is easier than providing an SQL formatted query string and less likely to be used for SQL injections. Though the PDO method `PDO::query($sql)` is still available, for maximum security we recommend the use of the new methods and familirizing yourself with the query strings `DataBaseTable::getData($query)` expects. Below are a few examples using simple strings to return single rows or small groups of data. Leaving this variable blank will return all the data in a given table.

```PHP
$table=new DataBaseTable('test');
$query=$table->getData("row_id:`= 1`");
$data=$query->fetch();
```

This simple example will retrieve all columns in the a row where the value of the column 'row_id' is equal to the integer 1 from the table named 'test'. Query operators are beyond the scope of this document, but work only with dates and integers. The final varialbe ($data) will hold an array of the data presented. See PHP's documentation on PDO::fetch for more details.

```PHP
$table=new DataBaseTable('test');
$query=$table->getData("row_name:`some text`");
$data=$query->fetch();
```

This example works like the above but instead of returning just the row where the 'row_id' is equal to the integer 1 it will return all rows where the value of the column `row_name` **contains** the text some text.

```PHP
$table=new DataBaseTable('test');
$query=$table->getData("some more text");
$data=$query->fetch();
```

If you have defined columns as FULLTEXT indices (see the documentation of your SQL server for more information) this will search those indices for "some more text" and return on the results **containing** that text.

Full documentation for this script is forthcoming and will included all the options for all methods including limiting columns returned by `DataBaseTable::getData()` as well as sorting and limiting those result. This should, however, be enough to get you started.