---
layout: page
title: HTML
order: 5
---

This package contains Twig template engine with flash messages and a useful Session class for WTF framework

<!-- vim-markdown-toc GFM -->

* [Install](#install)
* [Configure your app](#configure-your-app)
    - [Add new provider and middleware](#add-new-provider-and-middleware)
* [Documentation](#documentation)

<!-- vim-markdown-toc -->

## Install

```php
composer require wtf/html
```

## Configure your app

Create config file `html.php`:

```php
<?php

declare(strict_types=1);

$cache_dir = __DIR__.'/../cache';

return [
    'template_path' => __DIR__.'/../views/',
    'cache_path' => __DIR__.'/../cache',
];
```

**Optional**: create `csrf.php` config:

```php
<?php

declare(strict_types=1);

return [
    'failure_callable' => function ($request, $response, $next) { //@link https://github.com/slimphp/Slim-Csrf#handling-validation-failure
        $request = $request->withAttribute("csrf_status", false);
        return $next($request, $response);
    }
];
```

### Add new provider and middleware

1. `\Wtf\Html\Provider` into your providers list (`suit.php` config)
2. `session_middleware` and `csrf_middleware` into your middlewares list (`suit.php` config)

## Documentation

Plugin is currently extended with the following plugins. Instructions on how to use them in your own application are linked below.

* [Slim Twig](https://github.com/slimphp/Twig-View) - integration with [Twig template engine](https://twig.symfony.com)
* [Slim Flash](https://github.com/slimphp/Slim-Flash) - Flash messages implementation
* [Slim Twig Flash](https://github.com/kanellov/slim-twig-flash) - Integration of slim flash messages with Twig
* [Slim CSRF](https://github.com/slimphp/Slim-Csrf) - Implementation of CSRF token protection
* [RKA Session](https://github.com/akrabat/rka-slim-session-middleware) - Implementation of Session middleware with OOP session class
