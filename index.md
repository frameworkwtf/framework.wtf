---
layout: default
---
# What the Framework

WTF Framework is a functional abstraction over [Slim Framework](https://slimframework.com).

As WTF has backward compatibility with Slim and does not break or change any of Slim concepts, you have to be familiar with Slim Framework. If not - **[read Slim docs](https://slimframework.com/docs)** first.

## Vision

WTF vision is to enable rapid development by defining bare minimum functionality and remaning as clean and lightweight as the Slim is.

Let's check the following example:

<table width="80%">
    <thead width="100%">
        <tr width="80%">
            <td width="50%"><b>WTF Framework</b></td>
            <td width="50%"><b>Slim Framework</b></td>
        </tr>
    </thead>
    <tbody>
        <tr width="80%">
            <td>
                <div class="language-php highlighter-rouge" style="width: 90% !important"><div class="highlight"><pre class="codehilite"><code><span class="cp">&lt;?php</span>
<span class="k">use</span> <span class="nx">\Psr\Http\Message\ServerRequestInterface</span> <span class="k">as</span> <span class="nx">Request</span><span class="p">;</span>
<span class="k">use</span> <span class="nx">\Psr\Http\Message\ResponseInterface</span> <span class="k">as</span> <span class="nx">Response</span><span class="p">;</span>

<span class="k">require</span> <span class="s1">'vendor/autoload.php'</span><span class="p">;</span>

<span class="nv">$app</span> <span class="o">=</span> <span class="k">new</span> <span class="nx">\Wtf\App</span><span class="p">;</span>
<span class="nv">$app</span><span class="o">-&gt;</span><span class="na">get</span><span class="p">(</span><span class="s1">'/hello/{name}'</span><span class="p">,</span> <span class="k">function</span> <span class="p">(</span><span class="nx">Request</span> <span class="nv">$request</span><span class="p">,</span> <span class="nx">Response</span> <span class="nv">$response</span><span class="p">,</span> <span class="k">array</span> <span class="nv">$args</span><span class="p">)</span> <span class="p">{</span>
    <span class="nv">$name</span> <span class="o">=</span> <span class="nv">$args</span><span class="p">[</span><span class="s1">'name'</span><span class="p">];</span>
    <span class="nv">$response</span><span class="o">-&gt;</span><span class="na">getBody</span><span class="p">()</span><span class="o">-&gt;</span><span class="na">write</span><span class="p">(</span><span class="s2">"Hello, </span><span class="nv">$name</span><span class="s2">"</span><span class="p">);</span>

    <span class="k">return</span> <span class="nv">$response</span><span class="p">;</span>
<span class="p">});</span>
<span class="nv">$app</span><span class="o">-&gt;</span><span class="na">run</span><span class="p">();</span>
                </code></pre></div></div>
            </td>
            <td>
                <div class="language-php highlighter-rouge" style="width: 90% !important"><div class="highlight"><pre class="codehilite"><code><span class="cp">&lt;?php</span>
<span class="k">use</span> <span class="nx">\Psr\Http\Message\ServerRequestInterface</span> <span class="k">as</span> <span class="nx">Request</span><span class="p">;</span>
<span class="k">use</span> <span class="nx">\Psr\Http\Message\ResponseInterface</span> <span class="k">as</span> <span class="nx">Response</span><span class="p">;</span>

<span class="k">require</span> <span class="s1">'vendor/autoload.php'</span><span class="p">;</span>

<span class="nv">$app</span> <span class="o">=</span> <span class="k">new</span> <span class="nx">\Slim\App</span><span class="p">;</span>
<span class="nv">$app</span><span class="o">-&gt;</span><span class="na">get</span><span class="p">(</span><span class="s1">'/hello/{name}'</span><span class="p">,</span> <span class="k">function</span> <span class="p">(</span><span class="nx">Request</span> <span class="nv">$request</span><span class="p">,</span> <span class="nx">Response</span> <span class="nv">$response</span><span class="p">,</span> <span class="k">array</span> <span class="nv">$args</span><span class="p">)</span> <span class="p">{</span>
    <span class="nv">$name</span> <span class="o">=</span> <span class="nv">$args</span><span class="p">[</span><span class="s1">'name'</span><span class="p">];</span>
    <span class="nv">$response</span><span class="o">-&gt;</span><span class="na">getBody</span><span class="p">()</span><span class="o">-&gt;</span><span class="na">write</span><span class="p">(</span><span class="s2">"Hello, </span><span class="nv">$name</span><span class="s2">"</span><span class="p">);</span>

    <span class="k">return</span> <span class="nv">$response</span><span class="p">;</span>
<span class="p">});</span>
<span class="nv">$app</span><span class="o">-&gt;</span><span class="na">run</span><span class="p">();</span>
                </code></pre></div></div>
            </td>
        </tr>
    </tbody>
</table>

Looks similar, huh?

## The difference

* [x] Gitlab CI/CD out-of-the box (no config)
* [x] Dockerization out-of-the box (no config)
* [x] [Crash reporting](https://sentry.io) and [profiling](https://blackfire.io) out-of-the box (nearly zero configuration)
* [x] Easily configurable
* [x] Service layer with automated injection into context (through [Pimple](http://pimple.symfony.com))

Basicly any plugin, library, etc. (3rdParty) is integrated with WTF trhough the Service Provider layer.

There are serveral service providers available:

1. `wtf/orm` - ORM library based on [medoo](https://medoo.in) with [phinx](https://phinx.org) migrations and seeds
2. `wtf/html` - template library based on [Twig](https://twig.symfony.com) which includes flash messages, CSRF and many other useful stuff
3. `wtf/rest` - helper toolset for building REST APIs, eg: jwt integration
4. `wtf/auth` - authorization and authentication library which provides flexible way to define and use:
* - JWT auth
* - Session auth
* - Cookie auth
* - Social auth (coming soon)
* - Role-Based Access Control
* - Password manipulation

## > [Quick start](/quickstart)
