Mongodb DS for Cakephp
========

## Installing via composer

Install [composer](http://getcomposer.org) and run:

```bash
composer require nacer/mongodbcakephp dev-master
```

## Enable Plugin

```php 
// open src/Application.php
// then add plugin like below

public function bootstrap()
{
    $this->addPlugin('Nacer/Mongodbcakephp');
}

```
Before of CakePHP 3.7 (3.5.* & 3.6.*)

```php 
// at the end of this file config/bootstrap.php, add the code bellow

Plugin::load('Nacer/Mongodbcakephp');

```

## Fill Default Datasource using mongodb auth infos

```php
// In config/app.php file, replace the default datasource using mongodb like below
 'Datasources' => [
    'default' => [
        'className' => 'Nacer\Mongodbcakephp\Database\Connection',
        'driver' => 'Nacer\Mongodbcakephp\Database\Driver\Mongodb',
        'persistent' => false,
        'host' => 'localhost',
        'port' => 27017,
        'login' => '',
        'password' => '',
        'database' => 'devmongo',
        'ssh_host' => '',
        'ssh_port' => 22,
        'ssh_user' => '',
        'ssh_password' => '',
        'ssh_pubkey_path' => '',
        'ssh_privatekey_path' => '',
        'ssh_pubkey_passphrase' => ''
    ],
],
```

### SSH tunnel variables (starting with 'ssh_')
If you want to connect to MongoDB using a SSH tunnel, you need to set additional variables in your Datasource. Some variables are unnecessary, depending on how you intend to connect. IF you're connecting using a SSH key file, the ```ssh_pubkey_path``` and ```ssh_privatekey_path``` variables are necessary and the ```ssh_password``` variable is unnecessary. If you're connecting using a text-based password (which is **not** a wise idea), the reverse is true. The function needs, at minimum, ```ssh_host```, ```ssh_user``` and one method of authentication to establish a SSH tunnel.

## Models
After that, you need to load Nacer\Mongodbcakephp\ORM\Table in your tables class:

```php
//src/Model/Table/YourTable.php

use Nacer\Mongodbcakephp\ORM\Table;

class CategoriesTable extends Table {

}
```

## Observations

The function find() works only in the old fashion way.
So, if you want to find something, you to do like the example:

```php
$this->Categories->find('all', ['conditions' => ['name' => 'teste']]);
$this->Categories->find('all', ['conditions' => ['name LIKE' => 'teste']]);
$this->Categories->find('all', ['conditions' => ['name' => 'teste'], 'limit' => 3]);
```

You can also use the advanced conditions of MongoDB using the `MongoDB\BSON` namespace

```php
$this->Categories->find('all', ['conditions' => [
    '_id' => new \MongoDB\BSON\ObjectId('5a7861909db0b47d605c3865'),
    'foo.bar' => new \MongoDB\BSON\Regex('^(foo|bar)?baz$', 'i')
]]);
```

## LICENSE

[The MIT License (MIT) Copyright (c) 2013](http://opensource.org/licenses/MIT)
