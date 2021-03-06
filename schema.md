# Schema Builder

- [Introduction](#introduction)
- [Creating & Dropping Tables](#creating-and-dropping-tables)
- [Adding Columns](#adding-columns)
- [Renaming Columns](#renaming-columns)
- [Dropping Columns](#dropping-columns)
- [Checking Existence](#checking-existence)
- [Adding Indexes](#adding-indexes)
- [Dropping Indexes](#dropping-indexes)
- [Storage Engines](#storage-engines)

<a name="introduction"></a>
## Introduction

The Laravel `Schema` class provides a database agnostic way of manipulating tables. It works well with all of the databases supported by Laravel, and has a unified API across all of these systems.

<a name="creating-and-dropping-tables"></a>
## Creating & Dropping Tables

To create a new database table, the `Schema::create` method is used:

	Schema::create('users', function($table)
	{
		$table->increments('id');
	});

The first argument passed to the `create` method is the name of the table, and the second is a `Closure` which will receive a `Blueprint` object which may be used to define the new table.

To specify which connection the schema operation should take place on, use the `Schema::connection` method:

	Schema::connection('foo')->create('users', function($table)
	{
		$table->increments('id'):
	});

To drop a table, you may use the `Schema::drop` method:

	Schema::drop('users');

	Schema::dropIfExists('users');

<a name="adding-columns"></a>
## Adding Columns

To update an existing table, we will use the `Schema::table` method:

	Schema::table('users', function($table)
	{
		$table->string('email');
	});

The table builder contains a variety of column types that you may use when building your tables:

Command  | Description
------------- | -------------
`$table->increments('id');`  |  Incrementing ID to the table (primary key).
`$table->string('email');`  |  VARCHAR equivalent column
`$table->string('name', 100);`  |  VARCHAR equivalent with a length
`$table->integer('votes');`  |  INTEGER equivalent to the table
`$table->float('amount');`  |  FLOAT equivalent to the table
`$table->decimal('amount', 5, 2);`  |  DECIMAL equivalent with a precision and scale
`$table->boolean('confirmed');`  |  BOOLEAN equivalent to the table
`$table->date('created_at');`  |  DATE equivalent to the table
`$table->dateTime('created_at');`  |  DATETIME equivalent to the table
`$table->time('sunrise');`  |  TIME equivalent to the table
`$table->timestamp('added_on');`  |  TIMESTAMP equivalent to the table
`$table->timestamps();`  |  Adds **created\_at** and **updated\_at** columns
`$table->softDeletes();`  |  Adds **deleted\_at** column for soft deletes
`$table->text('description');`  |  TEXT equivalent to the table
`$table->binary('data');`  |  BLOB equivalent to the table
`$table->enum('choices', array('foo', 'bar'));` | ENUM equivalent to the table
`->nullable()`  |  Designate that the column allows NULL values
`->default($value)`  |  Declare a default value for a column
`->unsigned()`  |  Set INTEGER to UNSIGNED

If you are using the MySQL database, you may use the `after` method to specify the order of columns:

**Using After On MySQL**

	$table->string('name')->after('email');

<a name="renaming-columns"></a>
## Renaming Columns

To rename a column, you may use the `renameColumn` method on the Schema builder:

**Renaming A Column**

	Schema::table('users', function($table)
	{
		$table->renameColumn('from', 'to');
	});

> **Note:** Renaming `enum` column types is not supported.

<a name="dropping-columns"></a>
## Dropping Columns

**Dropping A Column From A Database Table**

	Schema::table('users', function($table)
	{
		$table->dropColumn('votes');
	});

**Dropping Multiple Columns From A Database Table**

	Schema::table('users', function($table)
	{
		$table->dropColumn('votes', 'avatar', 'location');
	});

<a name="checking-existence"></a>
## Checking Existence

You may easily check for the existence of a table or column using the `hasTable` and `hasColumn` methods:

**Checking For Existence Of Table**

	if (Schema::hasTable('users'))
	{
		//
	}

**Checking For Existence Of Columns**

	if (Schema::hasColumn('users', 'email'))
	{
		//
	}

<a name="adding-indexes"></a>
## Adding Indexes

The schema builder supports several types of indexes. There are two ways to add them. First, you may fluently define them on a column definition, or you may add them separately:

**Fluently Creating A Column And Index**

	$table->string('email')->unique();

Or, you may choose to add the indexes on separate lines. Below is a list of all available index types:

Command  | Description
------------- | -------------
`$table->primary('id');`  |  Adding a primary key
`$table->primary(array('first', 'last'));`  |  Adding composite keys
`$table->unique('email');`  |  Adding a unique index
`$table->index('state');`  |  Adding a basic index

<a name="dropping-indexes"></a>
## Dropping Indexes

To drop an index you must specify the index's name. Laravel assigns a reasonable name to the indexes by default. Simply concatenate the table name, the names of the column in the index, and the index type. Here are some examples:

Command  | Description
------------- | -------------
`$table->dropPrimary('users_id_primary');`  |  Dropping a primary key from the "users" table
`$table->dropUnique('users_email_unique');`  |  Dropping a unique index from the "users" table
`$table->dropIndex('geo_state_index');`  |  Dropping a basic index from the "geo" table

<a name="storage-engines"></a>
## Storage Engines

To set the storage engine for a table, set the `engine` property on the schema builder:

    Schema::create('users', function($table)
    {
        $table->engine = 'InnoDB';

        $table->string('email');
    });
