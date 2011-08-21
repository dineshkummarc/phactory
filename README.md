# Phactory

A PHP library for creating data for tests. Designed for simplicity
and extensibility.

## Usage

Define factories like so:

```php
<?php

class UserPhactory
{
	public function blueprint()
	{
		return array(
			'name' => 'User #{sn}',
			'activated' => true,
		);
	}

	public function admin()
	{
		return array(
			'isadmin' => true,
		);
	}
}

```

Then use them:

```php
<?php

$user = Phactory::user();

echo $user->name; // 'User 0001'
echo $user->activated ? 'true' : 'false'; // 'true'

$admin = Phactory::user('admin');

echo $user->name; // 'User 0002'
echo $user->activated ? 'true' : 'false'; // 'true'
echo $user->isadmin ? 'true' : 'false'; // 'true'

```

That's just the basics of what Phactory allows you to do, fixtures, dependencies
and relationships are also supported, read the wiki documentation for more information.

## What it doesn't do

Phactory doesn't know about your database or ORM, this is by design. Rather than trying
to support every ORM out there Phactory is designed to be easily extended to support
whatever it needs to. e.g. using our above UserPhactory

```php
<?php

class User {}

class MyCustomBuilder extends \Phactory\Builder
{
	protected function to_object($name, $values)
	{
		$object = new $name;

		foreach ($values as $key => $value)
			$object->$key = $value;

		return $object;
	}
}

Phactory::builder(new MyCustomBuilder);

$user = Phactory::user();

echo get_class($user); // 'User'

```

## Status

v1.1.1 Solid test coverage is in place, just type `phpunit` within your phactory directory.

## Installing

```
git clone git://github.com/rbone/phactory.git

<?php

require 'phactory/bootstrap.php';

```