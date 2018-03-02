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
* [Usage](#usage)

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
];
```

### Add new provider and middleware

1. `\Wtf\Auth\Provider` into your providers list (`suit.php` config)


## Usage

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

Result of `login()` different for each auth type:

* **Cookie**: will return cookie object, you should set it to response: `$response = \Dflydev\FigCookies\FigResponseCookies::set($response, $cookie);`
* **JWT**: will return jwt token string, you can return in in response: `$response->withJson(['token' => $token]);`
* **Session**: will return user entity object

```php
/**
 * Check if current user is logged in.
 * @return bool
 */
public function isLoggedIn(): bool
```

```php
/**
 * Get current user.
 * @return null|Root
 */
public function getUser(): ?Root
```
