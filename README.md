# Hydrahon

![Hydrahon banner](https://cloud.githubusercontent.com/assets/956212/7947360/e36d75ea-097c-11e5-89c0-be7b56bbf5ca.png)

Hydrahon is a query builder, and only a query builder. It does not contain a PDO wrapper or something. I'ts build to add query building into existing systems without implementing an entire new Database layer.

[![Build Status](https://travis-ci.org/ClanCats/Hydrahon.svg?branch=master)](https://travis-ci.org/ClanCats/Hydrahon)
[![Packagist](https://img.shields.io/packagist/dt/clancats/hydrahon.svg)]()
[![Packagist](https://img.shields.io/packagist/l/clancats/hydrahon.svg)]()
[![GitHub release](https://img.shields.io/github/release/clancats/hydrahon.svg)]()

## Status

**This library is currently in work and not stable**

 - [x] SQL query structure
 - [x] SQL select query builder
 - [x] Mysql select query translator
 - [ ] SQL insert query builder and translator
 - [ ] SQL update query builder and translator
 - [ ] SQL delete query builder and translator

## Installation

Hydrahon follows `PSR-4` autoloading and can be installed using composer:

```
$ composer require 'clancats/hydrahon:dev-master'
```

## Usage

### Creating hydrahon builder

Again Hydrahon is **not** build as a database library, it's just a query builder. In this example im going to show you a dead simple PDO mysql implementation.

```php 
$connection = new PDO('mysql:host=localhost;dbname=my_database', 'username', 'password');

$hydrahon = new \ClanCats\Hydrahon\Builder('mysql', function($query, $queryString, queryParameters) use($connection)
{
	$statement = $connection->prepare($queryString);
    $statement->execute($queryParameters);

    if ($query instanceof \ClanCats\Hydrahon\Query\Sql\Select)
    {
    	return $statement->fetchAll(\PDO::FETCH_ASSOC);
    }
});
```

### SQL query builder

#### Select 

##### Basics 

```php
$hydrahon->table('users')->select();
```

---

```php
// ignore this...

$hydrahon = new \ClanCats\Hydrahon\Builder('mysql', function( $query, $translated )
{
	// this is the callback function everytime executed when
	// a query is run here you can implement your logic
	list( $queryString, $queryParameters ) = $translated;

	echo "MySQL query: ".$queryString." with parameters: ".print_r( $queryParameters, true );
});

$hydrahon->notes->insert( ['title' => 'Hello'] );

$hydrahon->notes->select(['id','title'])->where( 'title', 'like', 'H%' )->get();

$hydrahon->table('other_db.notes')->find(1);

$hydrahon->table('notes')->delete()->where('created', '<', time() - 60 * 60 * 24 );
```
