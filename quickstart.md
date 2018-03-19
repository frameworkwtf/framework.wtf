---
layout: page
title: Quick Start
order: 2
---

<!-- vim-markdown-toc GFM -->

+ [Simple App](#simple-app)
    * [How to use magic?](#how-to-use-magic)
+ [Advanced usage](#advanced-usage)
    * [Create new project with skeleton](#create-new-project-with-skeleton)
    * [Configure routes](#configure-routes)
    * [Install addtional packages](#install-addtional-packages)
        - [HTML](#html)
        - [Find out more](#find-out-more)
    * [Docker](#docker)

<!-- vim-markdown-toc -->

# Simple App

Slim framework examples

Just use Slim "Hello world":

```php
<?php
use \Psr\Http\Message\ServerRequestInterface as Request;
use \Psr\Http\Message\ResponseInterface as Response;

require 'vendor/autoload.php';

$app = new \Wtf\App;
$app->get('/hello/{name}', function (Request $request, Response $response) {
    $name = $request->getAttribute('name');
    $response->getBody()->write("Hello, $name");

    return $response;
});
$app->run();
```

## How to use magic?

You need to pass path to your config folder in App constructor (like slim settings, see [application configuration](https://www.slimframework.com/docs/objects/application.html#application-configuration)), example:

```php

$config = [
    'config_dir' => __DIR__.'/config',
    'settings' => [
        'displayErrorDetails' => true,

        'logger' => [
            'name' => 'slim-app',
            'level' => Monolog\Logger::DEBUG,
            'path' => __DIR__ . '/../logs/app.log',
        ],
    ],
];
$app = new \Wtf\App($config);
```

And now, magic!

```php
$root = new \Wtf\Root($app->getContainer());
echo $root->config('app.site.name', 'production');
```

Place file `app.php` in `__DIR__.'/config'` folder (you passed it in App constructor, remember?):

```php
<?php return [
    'site' => [
        'name' => 'WTF Example',
        'url' => 'https://example.com',
        'parent' => 'https://slimframework.com',
    ],
];
```

And so on :)

# Advanced usage

## Create new project with skeleton

Pre-made skeleton with docker and easy-to-use structure

```bash
composer create-project wtf/skeleton
```

## Configure routes

[Documentation](https://www.slimframework.com/docs/objects/router.html#how-to-create-routes)

By default, route groups configured in `app/config/routes/` dir. Each file is a separate route group.

Example: if you want to create admin panel for your site, just place file `app/config/routes/admin.php` and add routes in it - all defined routes will have the prefix `http://your.site/admin`

## Install addtional packages

By default, the skeleton has only Core module installed, if you want more - feel free to add any library you need, but WTF has some pre-configured "bundles", check them out on [GitHub](https://github.com/frameworkwtf)

### HTML

If you want to generate HTML on backend side, just follow these steps and you will get [Twig](https://twig.symfony.com/), sessions and flash messages pre-configured and integrated in Slim framework.

* Install package:

```bash
composer require wtf/html
```

* Add configuration file `html.php` in your config dir (by default, `app/config`):

```php
<?php

declare(strict_types=1);

return [
    'template_path' => __DIR__.'/../views/',
    'cache_path' => __DIR__.'/../cache/views',
];
```

* Add provider `\Wtf\Html\Provider` into your `suit.php` config file (_suit.providers_)
* Add middleware `session_middleware` into your `suit.php` config file (_suit.middlewares_)

### Find out more

on [GitHub](https://github.com/search?utf8=✓&q=topic:wtf-module)

## Docker

WTF has predefined docker configs for quick start. You need to have Docker CE on your target machine (laptop,server,etc.) to run it

[Read more]({{ site.baseurl }}/docker)

Example (just for development):

```bash
docker run -it -p 80:8080 -v `pwd`:/var/www frameworkwtf/docker
```

PS: Docker image can be used in production, but you should set production mode via env var
