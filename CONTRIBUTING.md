# Contributing

For new additions or changes to the guide, create a branch and submit a Pull Request.
Only add/change 1 style guide rule per Pull Request.
The Pull Request serves as a place to discuss and refine the additions/changes.

## Format

Use [Github Flavoured Markdown](https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown).

Each rule should have the following format:

```markdown
## Rule name
Short summary of the rule to provide context.

### Why?
* list
* of
* benefits

### How?
Instructions with code examples.
```


### Rule name

Use short and descriptive rule names as 2nd level heading:

```markdown
## Avoid `.ready()`
```

### Summary
  
Put a short summary directly after the rule name to provide context. When referencing jQuery API methods, link them to jQuery's docs:

```markdown
jQuery's [`.ready()`](https://api.jquery.com/ready/) ensures your script is not executed before the DOM is ready. [...]
```

### The *Why?*

Describe why the rule exists. What purpose does it serve? Bullet lists are often most effective:

```markdown
### Why?

* Using `.ready()` means scripts have been loaded too early. Therefore defer loading instead.
* Script loading blocks page rendering. Therefore script loading should be defered. 
```

### The *How?*

Describe how (and how not) to apply the rule:

```markdown
### How?

Defer script loading by placing scripts just before the closing `</body>` tag or using the [`defer` attribute](https://developer.mozilla.org/en/docs/Web/HTML/Element/script#attr-defer):
```

### Code snippets

When using code snippets:
* use <code>```html</code> for highlighted markup examples. 
* use <code>```javascript</code> for highlighted script examples.
* use `<!-- recommended -->` and `/* recommended */` or `<!-- avoid -->` and `/* avoid */` at the start of the snippet to indicate if it's a good or bad practice.

For example:

```html
<!-- recommended: load in tail -->
<body>
   ...
	<script src="path/to/jquery.min.js"></script>
	<script>/* DOM is ready, use jQuery directly */</script>
</body>
```

```html
<!-- avoid: -->
<head>
	...
	<script src="path/to/jquery.min.js"></script>
	<script>jQuery.ready(function(){ /* ... */ });</script>
	<!-- same applies to external script containing `.ready()` -->
</head>
```

### Back to Table of Contents

Add a [↑ back to Table of Contents](README.md#table-of-contents) link at the end of each rule, so the reader can easily navigate back:

```markdown
[↑ back to Table of Contents](#table-of-contents)
```
