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
* [Assign `jQuery` to `$`](#assign-jquery-to-)


## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.


## Assign `jQuery` to `$`

## Why?

* Assigning `jQuery` to `$` is a common practice, which developers are familiair to.
* Assigning `jQuery` to `$` within a scope, avoids unexpected conflict with other scripts using `$`.
* The jQuery documentation and other resources typically use `$`.

## How?

Explicitly assign `jQuery` to `$` within a scope. When using a module loader (like CommonJS) assign it directly to a variable named `$`. Otherwise use an IIFE (immediately-invoked function expression):

```javascript
/* recommended when using module loader, like CommonJS: */
const $ = require('jquery');
// use jQuery as $
```

```javascript
/* recommended: use an IIFE */
(function($){
	// use jQuery as $
}(jQuery));
```

[↑ back to Table of Contents](#table-of-contents)
