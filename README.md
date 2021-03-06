# Aliases module for Kohana Framework

This module allows you to make useful and beautiful URLs for your service.

You don't need more `/user/<id>` or `/article/<id>` cursors in routes. Now you can use simply `/donald` and `/victory` or `/pokemon-go` like addresses for different resources.

## User Guide

Article describing this HMVC feature placed on our website [https://ifmo.su/alias-system](https://ifmo.su/alias-system).

### Installing

Firstly you need to copy this module to your project. You can download this repository and place its files to the `/modules` directory or attach it as a submodule. 

```shell
git submodule add https://github.com/codex-team/kohana-aliases modules/aliases
```

To enable module push it to `Kohana::modules()` array in `bootstrap.php`

```php
Kohana::modules(array(
    'aliases' => MODPATH . 'aliases', // Aliases for URLs
    ...
));
```

### Defining project's entities

In `classes` directory create a subdirectory `Aliases` with file `Controller.php`. You can copy [classes/Kohana/Aliases/Controller.php](classes/Kohana/Aliases/Controller.php).

Create constants for your site's entities and add them to Controllers map.

```php
const ARTICLE   = 1;
const USER      = 2;
...

const MAP = array(
    self::ARTICLE   => 'Articles',
    self::USER      => 'Users',
    ...
);
```

It means that entity `ARTICLE` will be handled by controller with name `Articles`.

#### Subcontrollers

Aliases module suggest you two types of subcontrollers:

- `Index` for showing entities
- `Modify` for do anything else with them

##### Example

If you have entity `User`, then create two controllers

1. To show user by uri `/alice` or `/bob`:

`Controller/Users/Index.php` with action `action_show`

2. To do any editions as adding, deleting. Uri: `/my-great-article/edit` or `/not-a-good-user/ban`

`Controller/Users/Modify.php` with all other actions e.g. `action_edit`, `action_ban`, `action_delete`.

All you need after is to include aliases creation and updating methods into your logic.

### Set system routes

If you want to set up system routes for your site or block some which shouldn't be allowed to use as alias. Then use [config/system-aliases.php](config/system-aliases.php) file. Lock any count of system URIs by adding them to the array.

### Database

Migrations for Aliases table in MySQL database migrations are in the [migrations/Aliases.sql](migrations/Aliases.sql) file.

### Create a new alias

```php
$alias         = Model_Aliases::generateUri($uri);
$resource_type = Aliases_Controller::ARTICLE;       // your own resource's type such as user, article, category and other
$resource_id   = 12345;

$article->uri = Model_Aliases::addAlias($alias, $resource_type, $resource_id);
```

### Update alias

```php
$resource_id   = $article->id;
$old_uri       = $article->uri;
$new_uri       = Model_Aliases::generateUri($uri);
$resource_type = Aliases_Controller::ARTICLE;

$article->uri = Model_Aliases::updateAlias($old_uri, $new_uri, $resource_type, $resource_id);
```

### Remove alias

```php
$hash = Model_Aliases::createRawHash($route);

Model_Aliases::deleteAlias($hash);
```

## Database and cache

You can create a `Model_DB_Aliases` class and create your own function to work with database e.g. for use caching system.

Copy file [classes/Model/DB/Aliases.php](classes/Model/DB/Aliases.php) and rewrite functions.

## Repository

<a href="https://github.com/codex-team/kohana-aliases/">https://github.com/codex-team/kohana-aliases/</a>

## About CodeX

We are small team of Web-developing fans consisting of IFMO students and graduates located in St. Petersburg, Russia.
Feel free to give us a feedback on <a href="mailto::team@ifmo.su">team@ifmo.su</a>
