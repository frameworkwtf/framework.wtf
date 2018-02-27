---
layout: page
title: Auth
order: 7
---

Authorization module that supports multiple auth methods (storages)

<!-- vim-markdown-toc GFM -->

* [Install](#install)
    - [Configure your app](#configure-your-app)
    - [Add new provider and middleware](#add-new-provider-and-middleware)
* [Usage](#usage)
    - [Basic](#basic)
    - [JWT auth](#jwt-auth)
        + [Login](#login)
        + [Get user](#get-user)
        + [Is Logged In](#is-logged-in)
    - [Session auth](#session-auth)
        + [Login](#login-1)
        + [Get user](#get-user-1)
        + [Is Logged In](#is-logged-in-1)
    - [Cookie auth](#cookie-auth)
        + [Login](#login-2)
        + [Get user](#get-user-2)
        + [Is Logged In](#is-logged-in-2)

<!-- vim-markdown-toc -->

## Install

```php
composer require wtf/auth
```

### Configure your app

Create config file `auth.php`:

```php
<?php

declare(strict_types=1);

return [
    'entity' => 'user', // user entity
    'storage' => \Wtf\Auth\Storage\Session::class, // can be Session, Cookie, JWT
    'repository' => \Wtf\Auth\Repository\User::class, // default user repository
    'password' => [
        'algo' => PASSWORD_DEFAULT,
        'options' => [],
    ],
];
```

### Add new provider and middleware

1. `\Wtf\Auth\Provider` into your providers list (`suit.php` config)


## Usage

### Basic

Each auth type uses following methods:

```php
/**
 * Login user
 * @param string $login User login (email, username, phone, etc)
 * @param string $password User password
 * @return mixed Can be JWT token string or user object
 */
public function login(string $login, string $password)
```

```php
/**
 * Check if current user is logged in.
 * @param mixed $storage custom storage, for JWT, Session, etc
 * @return bool
 */
public function isLoggedIn($storage = null): bool
```

```php
/**
 * Get current user.
 * @param mixed $storage custom storage, for JWT, Session, etc
 * @return null|Root
 */
public function getUser($storage = null): ?Root
```

But `$storage` arg value is different for each method

### JWT auth

**Requirements**: package `wtf/rest`  OR `tuupola/slim-jwt-auth` OR `firebase/php-jwt` should be installed

**$storage**: `ServerRequestInterface` ONLY if you don't have `wtf/rest` or `tuupola/slim-jwt-auth` packages installed, otherwise it's optional

Set `\Wtf\Auth\Storage\JWT::class` as storage in your `auth.php` config

#### Login

```php
<?php
//$root is any class in your app
$login = 'user@example.com';
$password = 'qwerty';
//Return JWT token string
$token = $root->auth->login($login, $password);
if($token) {
    return $response->withJson(['token' => $token]);
}

return $response->withStatus(401);
```

#### Get user

```php
<?php
//$root is any class in your app

$user = $root->auth->getUser(); //you should pass $request arg if you do NOT have wtf/rest or tuupola/slim-jwt-auth packages installed
if($user) {
    var_dump($user->getData());
}
```

#### Is Logged In

```php
<?php
//$root is any class in your app

$loggedIn = $root->auth->isLoggedIn(); //you should pass $request arg if you do NOT have wtf/rest or tuupola/slim-jwt-auth packages installed
if($loggedIn) {
    return $response->withStatus(200);
}

return $response->withStatus(401);
```

### Session auth

**Requirements**: php ext `session` should be installed

**$storage**: `null`

Set `\Wtf\Auth\Storage\Session::class` as storage in your `auth.php` config

#### Login

```php
<?php
//$root is any class in your app
$login = 'user@example.com';
$password = 'qwerty';

$user = $root->auth->login($login, $password);
if($user) {
    return $response->withStatus(200);
}

return $response->withStatus(401);
```

#### Get user

```php
<?php
//$root is any class in your app

$user = $root->auth->getUser();
if($user) {
    var_dump($user->getData());
}
```

#### Is Logged In

```php
<?php
//$root is any class in your app

$loggedIn = $root->auth->isLoggedIn();
if($loggedIn) {
    return $response->withStatus(200);
}

return $response->withStatus(401);
```

### Cookie auth

**Requirements**: package `dflydev/fig-cookies` should be installed

**$storage**: `ServerRequestInterface`

Set `\Wtf\Auth\Storage\Cookie::class` as storage in your `auth.php` config

#### Login

```php
<?php
//$root is any class in your app
$login = 'user@example.com';
$password = 'qwerty';

$cookie = $root->auth->login($login, $password);
if($cookie) {
    return \Dflydev\FigCookies\FigResponseCookies::set($response, $cookie);
}

return $response->withStatus(401);
```

#### Get user

```php
<?php
//$root is any class in your app

$user = $root->auth->getUser($request);
if($user) {
    var_dump($user->getData());
}
```

#### Is Logged In

```php
<?php
//$root is any class in your app

$loggedIn = $root->auth->isLoggedIn($request);
if($loggedIn) {
    return $response->withStatus(200);
}

return $response->withStatus(401);
```
