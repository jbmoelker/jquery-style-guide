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
* [Use .on() for event binding](#use-on-for-event-binding)
* [Consider native browser features](#consider-native-browser-features)
* [Consider lightweight alternative](#consider-lightweight-alternative)
* [Avoid `.ready()`](#avoid-ready)
* [Assign `jQuery` to `$`](#assign-jquery-to-)
* [Avoid `.show()`, `.hide()` and `.toggle()`](#avoid-show-hide-and-toggle)
* [Prefer promises over callbacks](#prefer-promises-over-callbacks)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Use [.on()](http://api.jquery.com/on/) for event binding

Methods like [`.click()`](http://api.jquery.com/click/) or [`.change()`](http://api.jquery.com/change/) are just alias for `.on('click')` and `.on('change')`. 

### Why?

* It's a way to keep consistency as all your events have the same signature.
* Avoiding using aliases let you trim the jQuery custom build. That way you reduce load/parse times and the file size.
 

### How?

``` javascript
/* avoid: .click() */
$('.todo-item').click(function(event) {});

/* recommended: .on() */
$('.todo-item').on('click', function() {});
```

[↑ back to Table of Contents](#table-of-contents)

## Consider native browser features

Since the release of jQuery many of its features now have a native browser equivalent. For example adding a class to an element can now be achieved via `element.classList.add(className)` instead of `$(element).addClass(className)`. Depending on the needs of your project you may be able to use only native browser features.

### Why?

* Native browser features are following the spec, making them future proof.
* Native browser features are closer to the metal, making them faster than jQuery.
* Developers who already know JavaScript don't need to learn the specifics of jQuery.

### How?

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


## Assign `jQuery` to `$`

### Why?

* Assigning `jQuery` to `$` is a common practice, which developers are familiair to.
* Assigning `jQuery` to `$` within a scope, avoids unexpected conflict with other scripts using `$`.
* The jQuery documentation and other resources typically use `$`.

### How?

Explicitly assign `jQuery` to `$` within a scope. When using a module loader (like CommonJS) assign it directly to a variable named `$`. Otherwise use an [IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) (immediately-invoked function expression):

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


## Avoid `.show()`, `.hide()` and `.toggle()`

JQuery let's you [`.show()`](http://api.jquery.com/show/), [`.hide()`](http://api.jquery.com/hide/) and [`.toggle()`](http://api.jquery.com/toggle/) elements. jQuery toggles an inline `display: none` to achieve this. 
HTML5 introduces a [new global attribute named `[hidden]`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/hidden), which is styled as `display: none` by default. It's better practice to use this native standard and toggle `[hidden]` instead of using  `.show()`, `.hide()` and `.toggle()`.

### Why?

* Avoid `.show()`, `.hide()` and `.toggle()` as they use inline styles, which are hard to overwrite.
* Prefer `[hidden]` as it semantically indicates an element is not yet, or no longer, relevant.
* Styles should be managed in (external) CSS stylesheets (not inlined) for separation of concerns.

### How?

Add, remove or toggle the `[hidden]` attribute instead of using `.show()`, `.hide()` or `.toggle()`.

Note: If you need to support pre HTML5 browsers use CSS to style `[hidden]` correctly:
```css
/* recommended: ensure hidden elements are not displayed in pre HTML5 browsers */
[hidden] { display: none !important; }
```

#### Show elements

```javascript
/* avoid: `.show()` elements */
$elements.show();

/* recommended: add `[hidden]` attribute */
$elements.attr('hidden', '');
```

#### Hide elements

```javascript
/* avoid: `.hide()` elements */
$elements.hide();

/* recommended: remove `[hidden]` attribute */
$elements.removeAttr('hidden');
```

#### Toggle elements

```javascript
/* avoid: `toggle()` elements */
$elements.toggle();
```

```javascript
/* recommended: toggle hidden attribute (with jQuery): */
// add `toggleHidden` functionality as jQuery plugin
$.fn.toggleHidden = function() {
	return this.each(function(index, element) {
		var $element = $(element);
		if ($element.attr('hidden')) {
			$element.removeAttr('hidden')
		} else {
			$element.attr('hidden', '');
		}
	});
};

// call `toggleHidden` on element:
$elements.toggleHidden();
```

```javascript
/* recommended: toggle hidden attribute (without jQuery): */
$elements.get().forEach(toggleHidden);

function toggleHidden(element) {
	if (element.hasAttribute('hidden')) {
		element.removeAttribute('hidden')
	} else {
		element.setAttribute('hidden', '');
	}
}
```

[↑ back to Table of Contents](#table-of-contents)

## Prefer promises over callbacks
A Promise represents a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers to an asynchronous action's eventual success value or failure reason.

### Why?
Use promises instead of callbacks to keep code more readable and future proof when handling asynchronous requests.
Promises can also be passed around so other modules can chain (.then) onto it, instead of complex callbacks inside callbacks (pyramid of doom) structures.

### How?

``` javascript
/* avoid: callbacks */
$.ajax('example.com/articles/1/author', {
  success: function(response) {},
  error: function(err) {}
});

/* recommended: use promise */
var request = $.ajax('example.com/articles/1/author');
request.then(function(response) {});
request.catch(function(err) {});
```

[↑ back to Table of Contents](#table-of-contents)
