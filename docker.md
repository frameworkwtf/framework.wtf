---
layout: page
title: Docker
order: 3
---
### How to use

Create `Dockerfile` in your poject root with following contents:

```Dockerfile
FROM frameworkwtf/docker

COPY . /var/www
```

Build and run it, that's all.

**PS**: It's already done in [wtf/skeleton](https://github.com/frameworkwtf/skeleton)

### Configuration (ENV vars)

* `PHP_OPCACHE_ENABLE` - Enable strict OPcache mode, default: `0`. Speed up app
* `APP_ENV` - Application environment, default: `dev`. Dev environment provides a lot of debug information and other goodies for devs
* `APP_RELEASE` - Application release for [Sentry releases](https://docs.sentry.io/learn/releases/), default: `local`. Should be configured automatically with CI
* `APP_HOST` - base app host, used in [REST module]({{ site.baseurl }}/rest), default: `localhost`. Should be changed on prod env to your own app host
* `APP_SECRET` - secret key, used in [REST module]({{ site.baseurl }}/rest), default: `qwerty`. Should be changed on prod env to your own secret key

### Integrated tools

1. **Swagger builder [swagger-php](https://github.com/zircote/swagger-php)** - If your project has swagger-php installed, it will be runned automatically on container start
2. **Phinx migrations and seed [phinx](http://phinx.org)** - If your project has phinx installed, migrations will be runned on container start. **IF** your **APP_ENV** env var **IS NOT** "prod", seeds will be runned after migrations
3. Console - If your project contains `app/public/console.php` file, you can simply run it with `console` from any part of the system. Very helpful when you want to add some cron scrits or run it via `docker exec`. No matter which console library you use
4. cron - [manual](https://wiki.alpinelinux.org/wiki/Alpine_Linux:FAQ#My_cron_jobs_don.27t_run.3F)
5. nginx
6. php-fpm
