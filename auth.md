---
layout: page
title: Auth
order: 7
repo: auth
---

Authorization module that supports multiple auth methods (storages)

<!-- vim-markdown-toc GFM -->

* [Install](#install)
    - [Configure your app](#configure-your-app)
    - [Add new provider and middleware](#add-new-provider-and-middleware)
    - [Add `rbac` to your routes](#add-rbac-to-your-routes)
* [Usage](#usage)
    - [Auth](#auth)
    - [Examples](#examples)

<!-- vim-markdown-toc -->

## Install

```bash
composer require wtf/auth
# for JWT
composer require wtf/rest
# for Cookie
composer require dflydev/fig-cookies

# for Session you should install PHP session module
```

### Configure your app

Create config file `app/config/auth.php`:

```php
<?php

declare(strict_types=1);

return [
    'entity' => 'user', // user entity
    'storage' => \Wtf\Auth\Storage\Session::class, // can be Session, Cookie, JWT
    'repository' => \Wtf\Auth\Repository\User::class, // default user repository
    'rbac' => [
        'defaultRole' => 'anonymous' //default unauthorized role
    ],
];
```

### Add new provider and middleware

1. `\Wtf\Auth\Provider` into your providers list (`app/config/suit.php` config)
2. `rbac_middleware` into your middlewares list (`app/config/suit.php` config)

### Add `rbac` to your routes

Example route group `home` (from skeleton):

```php
<?php

return [
    // ...
    'second' => [
        'pattern' => '/second', //route pattern, match: /home/second
        'methods' => ['GET', 'POST', 'PATCH'], //Allowed HTTP methods
        'rbac' => [
            'anonymous' => ['GET'], // allow unauthorized users (role: anonymous) to GET /home/second
            'user' => ['GET', 'POST'], // allow authorized users (role: user) to GET and POST /home/second
            'admin' => ['GET', 'POST', 'PATCH'] //allow admins (role: admin) to GET, POST and PATCH /home/second
        ],
    ],
];
```

## Usage


### Auth

Each auth type uses following methods:

```php
public function login(string $login, string $password)
```

Result of `login()` different for each auth type:

* **Cookie**: will return cookie object, you should set it to response: `$response = \Dflydev\FigCookies\FigResponseCookies::set($response, $cookie);`
* **JWT**: will return jwt token string, you can return in in response: `$response->withJson(['token' => $token]);`
* **Session**: will return user entity object

```php
public function isLoggedIn(): bool
```

```php
public function getUser(): ?Root
```

```php
public function logout(): void
```

### Examples

`$this->auth` available from any child of `\Wtf\Root` class

**Login**:

```php
$this->auth->login('you@email.com', 'password');
```

**Get user**:

```php
//without login user will be null, so let's login first
if(!$this->auth->isLoggedIn()) {
    $this->auth->login('you@email.com', 'password');
}
$this->auth->getUser();
```

**Logout**;
```php
$this->auth->logout();
```
