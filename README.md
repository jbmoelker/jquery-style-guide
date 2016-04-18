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
* [Consider native browser features](#consider-native-browser-features)
* [Consider lightweight alternative](#consider-lightweight-alternative)
* [Avoid `.ready()`](#avoid-ready)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.


## Consider native browser features

Since the release of jQuery many of its features now have a native browser equivalent. For example adding a class to an element can now be achieved via `element.classList.add(className)` instead of `$(element).addClass(className)`. Depending on the needs of your project you may be able to use only native browser features.

## Why?

* Native browser features are following the spec, making them future proof.
* Native browser features are closer to the metal, making them faster than jQuery.
* Developers who already know JavaScript don't need to learn the specifics of jQuery.

## How?

* Only apply Javascript after feature detection. Use a server-side rendered page as a basis. Enhance parts of a page only if the browser natively supports the required technology.
* Consult [platform.html5.org](https://platform.html5.org/) for overview of native browser technologies and [Can I use](http://caniuse.com/) and [Kangax tables](http://kangax.github.io/compat-table/es5/) for browser compatibility.
* Consult [you might not need jQuery](http://youmightnotneedjquery.com/) for native alternatives to jQuery features.

[↑ back to Table of Contents](#table-of-contents)


## Consider lightweight alternative

jQuery is the swiss army knive for DOM and event handling and much more. While jQuery offers a wide range of features, you might not need most of them in your project. Simply because you have little functionality or only modern browsers to support. In that case consider a lightweight alternative.

### Why?

* Lightweight alternatives have a smaller file size and can therefore be downloaded faster.
* Lightweight alternatives have less wrapper functionality which typically makes them faster.
* Some lightweight alternatives support only newer browsers and can therefore stay closer to native browser methods. This typically makes them more performant and easier to understand.
* Some lightweight alternatives mimic the jQuery API. Which means if you know jQuery, you know the alternative.

### How?

* Use a lightweight alternative like [Dominus](https://github.com/bevacqua/dominus#readme), [Shoestring](https://github.com/filamentgroup/shoestring#readme) or [Zepto](https://github.com/madrobby/zepto#readme).
* [Create a jQuery custom build](#create-a-custom-build) to only include the features you need.

[↑ back to Table of Contents](#table-of-contents)


## Avoid `.ready()`

jQuery's [`.ready()`](https://api.jquery.com/ready/) ensures your script is not executed before the DOM is ready. This is important because we typically want to access the DOM in our script. However, since the script can't be executed before the DOM is ready, a better practice is to defer script execution.

### Why?

* Using `.ready()` means scripts have been loaded too early. Therefore defer loading instead.
* Script loading blocks page rendering. Therefore script loading should be defered. 

### How?

Avoid loading scripts too early and waiting for the DOM to be ready using `.ready()`.
Defer script loading by placing scripts just before the closing `</body>` tag or using the [`defer` attribute](https://developer.mozilla.org/en/docs/Web/HTML/Element/script#attr-defer):

```html
<!-- recommended: load in tail -->
<body>
   ...
	<script src="path/to/jquery.min.js"></script>
	<script>/* DOM is ready, use jQuery directly */</script>
</body>
```

```html
<!-- recommended: defer script loading -->
<head>
	...
	<script defer src="path/to/jquery.min.js"></script>
	<script defer src="path/to/my-app.min.js"></script>
</head>
```

Note: Be aware [using `defer` in IE <= 9 can cause issues](https://github.com/h5bp/lazyweb-requests/issues/42). So consider your browser scope before using this setup.

```html
<!-- avoid: -->
<head>
	...
	<script src="path/to/jquery.min.js"></script>
	<script>jQuery.ready(function(){ /* ... */ });</script>
	<!-- same applies to external script containing `.ready()` -->
</head>
```

[↑ back to Table of Contents](#table-of-contents)
