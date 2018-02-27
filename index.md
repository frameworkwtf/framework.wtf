---
layout: default
order: 1
---

# What the ...?

## Concept

As a web developer, you need some infrastructure code for your applications. For example: dependency manager, router, DI container and so on.

We need such a tool, too. That's why we created wtf.

Main idea is pretty simple: we already implemented lots of infrastructure logic (config resolver, "magic system parent" and so on), we just need to bundle it in a separate package to use for any other projects without Ctr+C/Ctr+V

## Basic Requirements

1. We need a simple http request/response handler
2. We need good DI container
3. We are too lazy to implement it ourself
4. Symfony, Laravel and so on are too big for us and have lots of disadvantages.

Ok, **Slim Framework** is our (good) choise!

## Real requirements

1. Well, Slim is really good, but we need a tool for convenient config management. Result: `Config` class, adopted from [PHPixie 2.x](https://github.com/dracony/phpixie-core). Thank you, [@dracony](https://github.com/dracony) :)
2. Ok, but we wanna magic! We don't want to call `$app->getContainer()->get('something')` each time, it's too long. Result: `Root` class from [TiSuit](https://github.com/tisuit) (wtf framework "grandpa")
3. Hm... Ok, but I really need to extend parent `Root` class. Welcome, Pimple Providers.
4. And the final step: comfortable DI configuration (slim's `dependencies.php` is too ugly and unusable). Welcome `Provider` class, thank you, [Pimple](https://pimple.symfony.com)

## Conclusion

We've built a very flexible solution, which allows you to create business logic of your app, without worrying about infrastructure code (_note:_ 100% test coverage).

One more thing: `wtf/core` package is just a main component. You can use it without any other, just as you use slim framework itself, but with wtf you will have all these magic (very simple and predictable, in fact. It's not as Spring in Java :) things and easy to use classes in your app with full backward-compability with Slim framework itself.
