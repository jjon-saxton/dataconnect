# dataconnect
Extensions to PDO to make working with data in an SQL database easier and a bit more secure!

## Installation

In theory, installation is fairly easy and straightforward. Simply add the single file `database.inc.php` to your project and use php's `include` or `require` keywords to your main project file so you can use the classes contained within.

## Configuration

Before you can use the classes, however, you **must** configure the script to work with your database instance. At this time it only supports MySQL, but we will be working on supporting other databases in the future. An example `settings.ini` file is included for you in the root folder alongside this README. Comments are provided to help guide you through the processes and options, if applicable.