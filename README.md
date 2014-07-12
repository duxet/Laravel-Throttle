Laravel Throttle
================


[![Build Status](https://img.shields.io/travis/GrahamCampbell/Laravel-Throttle/master.svg?style=flat)](https://travis-ci.org/GrahamCampbell/Laravel-Throttle)
[![Coverage Status](https://img.shields.io/scrutinizer/coverage/g/GrahamCampbell/Laravel-Throttle.svg?style=flat)](https://scrutinizer-ci.com/g/GrahamCampbell/Laravel-Throttle/code-structure)
[![Quality Score](https://img.shields.io/scrutinizer/g/GrahamCampbell/Laravel-Throttle.svg?style=flat)](https://scrutinizer-ci.com/g/GrahamCampbell/Laravel-Throttle)
[![Software License](https://img.shields.io/badge/license-Apache%202.0-brightgreen.svg?style=flat)](LICENSE.md)
[![Latest Version](https://img.shields.io/github/release/GrahamCampbell/Laravel-Throttle.svg?style=flat)](https://github.com/GrahamCampbell/Laravel-Throttle/releases)


## Introduction

Laravel Throttle was created by, and is maintained by [Graham Campbell](https://github.com/GrahamCampbell), and is a rate limiter for [Laravel 4.1+](http://laravel.com). Feel free to check out the [change log](CHANGELOG.md), [releases](https://github.com/GrahamCampbell/Laravel-Throttle/releases), [license](LICENSE.md), [api docs](http://docs.grahamjcampbell.co.uk), and [contribution guidelines](CONTRIBUTING.md).


## Installation

[PHP](https://php.net) 5.4.7+ or [HHVM](http://hhvm.com) 3.1+, and [Composer](https://getcomposer.org) are required.

To get the latest version of Laravel Throttle, simply require `"graham-campbell/throttle": "~1.0"` in your `composer.json` file. You'll then need to run `composer install` or `composer update` to download it and have the autoloader updated.

Once Laravel Throttle is installed, you need to register the service provider. Open up `app/config/app.php` and add the following to the `providers` key.

* `'GrahamCampbell\Throttle\ThrottleServiceProvider'`

You can register the Throttle facade in the `aliases` key of your `app/config/app.php` file if you like.

* `'Throttle' => 'GrahamCampbell\Throttle\Facades\Throttle'`


## Configuration

Laravel Throttle supports optional configuration.

To get started, first publish the package config file:

    php artisan config:publish graham-campbell/throttle

There is one config option:

**Cache Driver**

This option (`'driver'`) defines the cache driver to be used. It may be the name of any driver set in app/config/cache.php. Setting it to null will use the driver you have set as default in app/config/cache.php. Please note that a driver that supports cache tags is required. The default value for this setting is `null`.


## Usage

**Throttle**

This is the class of most interest. It is bound to the ioc container as `'throttle'` and can be accessed using the `Facades\Throttle` facade. There are six public methods of interest.

The `'get'` method will create a new throttler class (a class that implements `Throttler\ThrottlerInterface`) from the 1-3 parameters that you pass to it. The first parameter is required and must either an instance of `\Illuminate\Http\Request`, or an associative array with two keys (`'ip'` should be the ip address of the user you wish to throttle and `'route'` should be the full url you wish to throttle, but actually, for advanced usage, may be any unique key you choose). The second parameter is optional and should be an `int` which represents the maximum number of hits that are allowed before the user hits the limit. The third and final parameter should be an `int` that represents the time the user must wait after going over the limit before the hit count will be reset to zero. Under the hood this method will be calling the make method on a throttler factory class (a class that implements `Factories\FactoryInterface`).

The other 5 methods all accept the same parameters as the `get` method. What happens here is we dynamically create a throttler class (or we automatically reuse an instance we already created), and then we call the method on it with no parameters. These 5 methods are `'attempt'`, `'hit'`, `'clear'`, `'count'`, and `'check'`. They are all documented bellow.

**Facades\Throttle**

This facade will dynamically pass static method calls to the `'throttle'` object in the ioc container which by default is the `Throttle` class.

**Throttler\ThrottlerInterface**

This interface defines the public methods a throttler class must implement. All 5 methods here accept no parameters.

The `'attempt'` method will hit the throttle (increment the hit count), and then will return a boolean representing whether or not the hit limit has been exceeded.

The `'hit'` method will hit the throttle (increment the hit count), and the will return `$this` so you can make another method call if you so choose.

The `'clear'` method will clear the throttle (set the hit count to zero), and the will return `$this` so you can make another method call if you so choose.

The `'count'` method will return the number of hits to the throttle.

The `'check'` method will return a boolean representing whether or not the hit limit has been exceeded.

**Throttler\CacheThrottler**

This class implements `Throttler\ThrottlerInterface` completely. This is the only throttler implementation shipped with this package, and in created by the `Factories\CacheFactory` class.

**Factories\FactoryInterface**

This interface defines the public methods a throttler factory class must implement. Such a class must only implement one method.

The `'make'` method will create a new throttler class (a class that implements `Throttler\ThrottlerInterface`) from the 1-3 parameters that you pass to it. It accepts the same parameters as the `'get'` method on the `Throttle` class. Please read the docs for that method for information on parameters and expected usage. This documentation of an internal interface is included for advanced users who may wish to write their own factory classes to make their own custom throttler classes.

**Factories\CacheFactory**

This class implements `Factories\FactoryInterface` completely. This is the only throttler implementation shipped with this package, and is responsible for creating the `Factories\CacheFactory` class. This class is only intended for internal use by the `Throttle` class.

**ThrottleServiceProvider**

This class contains no public methods of interest. This class should be added to the providers array in `app/config/app.php`. This class will setup ioc bindings.

**Filters**

You may put the `throttle` filter in front of your routes to throttle them. The filter can take up to two parameters. The two parameters are `limit` and `time`. It may be useful for you to take a look at the [source](https://github.com/GrahamCampbell/Laravel-Throttle/blob/master/src/filters.php) for this, read the [tests](https://github.com/GrahamCampbell/Laravel-Throttle/blob/master/tests/Functional/FilterTest.php), or check out Laravel's [documentation](http://laravel.com/docs/routing#route-filters) if you need to.

**Further Information**

Feel free to check out the [API Documentation](http://docs.grahamjcampbell.co.uk) for Laravel Throttle.

You may see an example of implementation in [Laravel Credentials](https://github.com/GrahamCampbell/Laravel-Credentials) and [Bootstrap CMS](https://github.com/GrahamCampbell/Bootstrap-CMS).


## License

Apache License

Copyright 2013-2014 Graham Campbell

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

 http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
