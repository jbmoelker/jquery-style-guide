# jQuery Style Guide

Opinionated *jQuery Style Guide* for teams by [De Voorhoede](https://twitter.com/devoorhoede).

## Purpose

This guide aims to improve the way your team uses [jQuery](http://jquery.com/). It helps you write code which is

* close to modern web technologies and best practices.
* easy to understand for developers / team members.
* easy to reuse (in other projects).
* more performant.
* avoids overuse of jQuery and needless vendor locking.


## Table of Contents

* [About jQuery](#about-jquery)
* [General](#general)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## General

When adding jQuery to your project always make sure you pick the latest version.
Currently 3.0 is the latest version which drops support for older browsers to reduce size and use more native JavaScript for the best performance.
Always check the browser scope of your project to see which version of jQuery fits best.

Creating a custom build can greatly reduce the size of the library, more information on how to create a custom build can be found on the [jQuery Github](https://github.com/jquery/jquery#how-to-build-your-own-jquery) page. In this guide there are a few suggestions of modules which can be omitted when creating a custom build.

Using a [linter](https://en.wikipedia.org/wiki/Lint_(software)), like [jQuery Lint](http://james.padolsey.com/javascript/jquery-lint/), helps with code consistency and can hint on best practices in your code. Check if there are linters for your preferred IDE to get the support directly inside your IDE. Webstorm has [build-in support](https://www.jetbrains.com/help/webstorm/2016.1/configuring-javascript-libraries.html) for jQuery and some other libraries.
