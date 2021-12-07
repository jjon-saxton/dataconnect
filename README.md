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