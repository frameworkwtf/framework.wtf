---
layout: page
title: REST
order: 6
repo: rest
---

<!-- vim-markdown-toc GFM -->

- [Install](#install)
- [Configure your app](#configure-your-app)
- [Add new provider and middleware](#add-new-provider-and-middleware)

<!-- vim-markdown-toc -->

### Install

```php
composer require wtf/rest
```

### Configure your app

Create config file `jwt.php`:

```php
<?php

declare(strict_types=1);

return [
	"path" => "/api",
	"passthrough" => ["/api/login"],
	"secret" => getenv('APP_SECRET'),
];
```

Documentation: [tuupola/slim-jwt-auth](https://github.com/tuupola/slim-jwt-auth/tree/3.x#usage)

### Add new provider and middleware

1. `\Wtf\Rest\Provider` into your providers list (`suit.php` config)
2. `jwt_middleware` into your middlewares list (`suit.php` config)
