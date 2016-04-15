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
* [Avoid using `.click()`, `.change()`, et al as event handlers]

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Avoid using `.click()`, `.change()`, et al as event handlers
### Why?
Using the .on() method instead can reduce memory usage and works on dynamically added elements.

Events added with .on() can also be namespaced. This make code easier to read and when removing the event you are sure you are not accidentally removing other event handlers.

### How?
``` javascript
// Bad
$('.todo-item').click(function(event) {
  // ...
});

// Good
$('.todo-item').on('click', function() {
  // ...
});

// Better
$('.todo-list').on('click', '.todo-item', function(event) {
  // ...
});
```

Using namespaced event handlers
``` javascript
// Adding namespaced event
$('.todo-item').on('click.todoItem', function(event) {
  // ...
});

// Remove namespaced event
$('.todo-item').off('click.todoItem');
```

[↑ back to Table of Contents](#table-of-contents)