---
layout: page
title: Auth
order: 7
---

Authorization module that supports multiple auth methods (storages)


<!-- vim-markdown-toc GFM -->

- [Install](#install)
- [Configure your app](#configure-your-app)
- [Add new provider and middleware](#add-new-provider-and-middleware)
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

### Install

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


### JWT auth

**Requirements**: package `wtf/rest`  OR `tuupola/slim-jwt-auth` OR `firebase/php-jwt` should be installed

Set `\Wtf\Auth\Storage\JWT::class` as storage in your `auth.php` config

#### Login

```php
<?php
//$root is any class in your app
$login = 'user@example.com';
$password = 'qwerty';

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
