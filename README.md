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
* [Detach elements when working with them](#detach-elements-when-working-with-them)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Detach elements when working with them
### Why?
Because updating the DOM is heavy on the client elements should be .detach() -ed when updating multiple things.

### How?
``` javascript
var todoItems = $('.todo-items');
var parent = todoItems.parent();

todoItems.detach(); 
// ... add/remove classes, attributes and/or content
 
parent.append(todoItems);
```

[↑ back to Table of Contents](#table-of-contents)
