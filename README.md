<p align="center">
    <a href="https://github.com/yiisoft" target="_blank">
        <img src="https://github.com/yiisoft.png" height="100px">
    </a>
    <h1 align="center">Yii Auth JWT</h1>
    <br>
</p>

[![Latest Stable Version](https://poser.pugx.org/yiisoft/auth-jwt/v/stable.png)](https://packagist.org/packages/yiisoft/auth-jwt)
[![Total Downloads](https://poser.pugx.org/yiisoft/auth-jwt/downloads.png)](https://packagist.org/packages/yiisoft/auth-jwt)
[![Build status](https://github.com/yiisoft/auth-jwt/workflows/build/badge.svg)](https://github.com/yiisoft/auth-jwt/actions)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/yiisoft/auth-jwt/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/yiisoft/auth-jwt/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/yiisoft/auth-jwt/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/yiisoft/auth-jwt/?branch=master)
[![Mutation testing badge](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fbadge-api.stryker-mutator.io%2Fgithub.com%2Fyiisoft%2Fauth-jwt%2Fmaster)](https://dashboard.stryker-mutator.io/reports/github.com/yiisoft/auth-jwt/master)
[![static analysis](https://github.com/yiisoft/auth-jwt/workflows/static%20analysis/badge.svg)](https://github.com/yiisoft/auth-jwt/actions?query=workflow%3A%22static+analysis%22)

The package provides JWT authentication method for [Yii Auth](https://github.com/yiisoft/auth/).

## Installation

The package could be installed with composer:

```
composer install yiisoft/auth-jwt
```

## General usage

1. Set jwt secret in your ```params.php``` config file:
```php
    'yiisoft/auth-jwt' => [
        'secret' => 'your-secret'
    ],
```

2. Setup definitions, required for ```\Yiisoft\Auth\Middleware\Authentication``` middleware in config, for example, in ```web.php```:
```php
    Yiisoft\Auth\Jwt\TokenManagerInterface::class => [
        '__class' => Yiisoft\Auth\Jwt\TokenManager::class,
        '__construct()' => [
            'secret' => $params['yiisoft/auth-jwt']['secret']
        ]
    ],

    Yiisoft\Auth\AuthenticationMethodInterface::class => [
        '__class' => Yiisoft\Auth\Jwt\JwtMethod::class
    ],
```
Don't forget to declare your implementations of ```\Yiisoft\Auth\IdentityInterface``` and ```\Yiisoft\Auth\IdentityRepositoryInterface``` also.

After that you can use ```Yiisoft\Auth\Middleware\Authentication``` middleware.

Read more about middlewares in the [middleware guide](https://github.com/yiisoft/docs/blob/master/guide/en/structure/middleware.md). 

Otherwise, you can configure Authentication middleware manuallly:
```php
$identityRepository = getIdentityRepository(); // \Yiisoft\Auth\IdentityRepositoryInterface

$tokenManager = $container->get(\Yiisoft\Auth\Jwt\TokenManagerInterface::class);

$authenticationMethod = new \Yiisoft\Auth\Jwt\JwtMethod($identityRepository, $tokenManager);

$middleware = new \Yiisoft\Auth\Middleware\Authentication(
    $authenticationMethod,
    $responseFactory, // PSR-17 ResponseFactoryInterface
    $failureHandler // optional, \Yiisoft\Auth\Handler\AuthenticationFailureHandler by default
);
```

## Unit testing

The package is tested with [PHPUnit](https://phpunit.de/). To run tests:

```php
./vendor/bin/phpunit
```

## Mutation testing

The package tests are checked with [Infection](https://infection.github.io/) mutation framework. To run it:

```php
./vendor/bin/infection
```

## Static analysis

The code is statically analyzed with [Psalm](https://psalm.dev/). To run static analysis:

```php
./vendor/bin/psalm
```
