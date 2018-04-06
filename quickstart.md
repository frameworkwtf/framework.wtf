---
layout: page
title: Quick Start
order: 2
---

<!-- vim-markdown-toc GFM -->

+ [Prerequisites](#prerequisites)
+ [Installation](#installation)
+ [Now let's REST](#now-lets-rest)
+ [Read Next](#read-next)

<!-- vim-markdown-toc -->

# Prerequisites

* [ ] Docker
* [ ] docker-compose
* [ ] Composer
* [ ] PHP ^7.1

# Installation

```bash
composer create-project wtf/skeleton your-app
```

This command creates the skeleton project with structure, shown below:

```bash
your-app
├── app                      # Main app dir
│   ├── cache                # Cache dir
│   ├── config               # Config dir
│   ├── migrations           # DB migrations dir
│   ├── public               # Public dir accessible from internet
│   ├── seeds                # DB seeds dir
│   ├── src                  # App source code
│   │   ├── Controller       # Controllers
│   │   ├── Controller.php   # Main app controller
│   │   ├── Entity           # Entities
│   │   ├── ErrorHandler.php # App error handler
│   │   ├── Middleware       # App middlewares
│   │   └── Provider.php     # App service provider
│   ├── tests                # App tests
│   └── vendor               # 3rdParty dependencies
├── composer.json            # package manager configuration
├── Dockerfile               # Docker configuration
├── .dockerignore            # List of ignored files in docker
├── env                      # Docker environments
│   ├── dev.yml              # Development, default
│   ├── prod.yml             # Production
│   └── qa.yml               # QA/Staging
├── .env                     # docker-compose env
├── .gitignore               # List of ignored files in git
├── .gitlab-ci.yml           # Gitlab CI/CD configuration
├── .php_cs.dist             # php-cs-fixer configuration
├── phpunit.xml.dist         # PHPUnit configuration
└── README.md                # README
```

# Now let's REST

Generate your first CRUD called `App`:

```bash
app/vendor/bin/wtf generate:crud App -c app/config/generator.php
```

You should see something like this in output:

```
WTF framework generator

Controller saved to ./app/src/Controller/App.php
Test saved to ./app/tests/App.php
Entity saved to ./app/src/Entity/App.php
Test saved to ./app/tests/App.php
Route saved to ./app/config/routes/app.php
Migration saved to ./app/migrations/20180406100619_init_app.php
```

Now, let's add `What the fuck? It works!` text in your controller:

1. Open `app/src/Controller/App.php`
2. Replace entire content in function `indexAction()` with `return $this->json(['What the fuck?' => 'It works!']);`

```php
public function indexAction(): ResponseInterface
{
    return $this->json(['What the fuck?' => 'It works!']);
}
```

And let's test it:

```bash
docker-compose up -d
curl http://localhost:8080/app
```

The result will be:

```json
{"error":{"message":null,"fields":[]},"count":1,"data":{"What the fuck?":"It works!"}}
```

That's all, folks!

# Read Next

{% assign pages = site.pages | sort: 'order' %}
{% for item in pages %}
{%- if item.title and item.title != "404 - Page not found" and item.title != page.title -%}
<[{{ item.title }}]({{ item.url}})>&nbsp;
{%- endif -%}
{% endfor %}<[API docs]({{ site.baseurl }}/api)>
