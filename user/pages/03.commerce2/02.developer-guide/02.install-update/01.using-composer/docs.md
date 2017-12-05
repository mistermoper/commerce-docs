---
title: Using Composer
taxonomy:
    category: docs
---

[Composer] is a tool for managing dependencies on the project level, a
project being your site or web application. It has become the de facto dependency
management tool for PHP. Composer allows PHP developers to easily build standalone
distributable libraries that can be shared and integrated by others. This is
possible in part by the PHP Framework Interoperability Group (FIG) and [PSR-4]
for autoloading of class files.

>  Dependency management is not a new concept and not alone to PHP. NPM for NodeJS,
>  Bower for front end libraries, Bundler/Gems for Ruby, PIP for Python, Maven for
>  Java and so forth.

If you’ve ever used [Drush] Make to download Drupal modules and themes, then
all of this sounds familiar. You can think of Composer as the more advanced
Drush Make that works for all PHP projects and packages. Compared to Drush Make,
Composer has the benefit of being able to recursively resolve dependencies
(downloading the dependencies of each dependency) and being able to detect conflicts.

## Why does Commerce need it?

Modern applications such as Drupal 8 consist of many classes, so it would be
impractical and costly to manually include each one. Instead, the application
includes one special class, called the autoloader which then automatically
includes other classes when they are first needed. When Composer runs, it
regenerates the autoloader, giving it the locations of the newly downloaded
dependencies.

Commerce utilizes various [libraries and dependencies](../../02.libraries-and-dependencies). Without Composer and the
generated class autoloader you cannot use Commerce. The libraries we depend on
will not be available, even if manually installed.

Composer also enables version constraints and prevents dependency conflicts.
Without Composer there is no way to tell our users “make sure you also update
this dependency as well.”

This also means less work for you.

## How to use it

### [composer.json]

The ``composer.json`` file defines required libraries, modules, themes, and
Drupal core to download. It allows you to run commands when Composer finishes
an operation, and more.

Here is an example from the `commerce_authnet` module.

```json
{
  "name": "drupal/commerce_authnet",
  "type": "drupal-module",
  "description": "Provides Commerce integration for Authorize.net.",
  "homepage": "http://drupal.org/project/commerce_authnet",
  "license": "GPL-2.0+",
  "require": {
    "drupal/commerce": "^2",
    "commerceguys/authnet": "dev-master"
  }
}
```

This specifies the project as being a Drupal module available at ``drupal/commerce_authnet``
with information about its license and homepage. It requires `commerce` of any
satisfiable version 2 release, and the development version of the `commerceguys/authnet` library.

>  Composer relies on semantic versioning, using `~` and `^` operators, or direct
>  release names (`2.0-beta3`.)
> 
>  Check out the Packagist Semver Checker to explore how version constraints work.
>  This link is for `drupal/core: ^8.3` [https://semver.mwl.be/#?package=drupal%2Fdrupal&version=%5E8.3&minimum-stability=dev](https://semver.mwl.be/#?package=drupal%2Fdrupal&version=%5E8.3&minimum-stability=dev)

### [composer.lock]

The `composer.lock` file is auto generated by Composer. It has information about
all the dependencies in the project, including ones your modules or themes depend
on.

The lock file contains information on how to fetch the dependency and its
dependency version constraints.

### [composer install]

The `composer install` command will download and install dependencies. The install command will install off of lock file. However, if no lock file is available it will act as the update command.

The command will regenerate the class autoloader.

### [composer update]

The `composer update` command resolves dependencies and generates the `composer.lock` file. The update command will update dependencies to their latest versions.

The command will regenerate the class autoloader.

### [composer require]

The `composer require` command adds a new dependency to your project. This will update the `composer.json` and `composer.lock` files and regenerate the class autoloader.

If the new dependency has any conflicts with other dependencies, such as incompatible shared dependencies, it will not install.

### [composer remove]

The `composer remove` command removes a dependency from your project. This will update the `composer.json` and `composer.lock` files and regenerate the class autoloader.

If the dependency is required by another package, it will not be removed.

## Links and resources

* [Managing Your Drupal Project with Composer](https://glamanate.com/blog/managing-your-drupal-project-composer), or the [slides version](https://docs.google.com/presentation/d/1PK9q2dBkGHfyEO76bEVpqS61wTgA0LGbru2PECiwUnk/edit?usp=sharing)
* [Drupal Commerce project template](https://github.com/drupalcommerce/project-base)
* [Drupal Composer project template](https://github.com/drupal-composer/drupal-project)
* [Platform.sh Drupal 8 + Composer template example](https://github.com/platformsh/platformsh-example-drupal8)
* [Amazee Labs Composer recipes](https://www.amazeelabs.com/en/blog/drupalcomposerrecipes)
* [Using Drupal + Composer project templates with Pantheon sites](https://pantheon.io/blog/using-composer-relocated-document-root-pantheon)
* [Using Composer - Drupal.org](https://www.drupal.org/docs/develop/using-composer)

[Composer]: https://getcomposer.org/
[PSR-4]: http://www.php-fig.org/psr/psr-4/
[Drush]: http://www.drush.org
[composer.json]: https://getcomposer.org/doc/04-schema.md
[composer.lock]: https://getcomposer.org/doc/01-basic-usage.md#composer-lock-the-lock-file
[composer install]: https://getcomposer.org/doc/03-cli.md#install
[composer update]: https://getcomposer.org/doc/03-cli.md#update
[composer require]: https://getcomposer.org/doc/03-cli.md#require
[composer remove]: https://getcomposer.org/doc/03-cli.md#remove