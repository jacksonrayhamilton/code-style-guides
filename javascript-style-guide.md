# JavaScript Style Guide

The following document outlines a style guide for JavaScript programs.

# Prelude

StoryCloud's JavaScript Style Guide extends Douglas Crockford's
[Code Conventions for the JavaScript Programming Language][]. Everything in that
style guide also applies to this style guide. Please familiarize yourself with
it.

[Code Conventions for the JavaScript Programming Language]: http://javascript.crockford.com/code.html

## Common Courtesy

- Prefer ECMAScript 5.
  - We don't support ECMAScript 3-only browsers, so constructs like `typeof
    variable === 'undefined'` can be replaced with `variable === undefined`, et
    al.
  - Open-source code should use ECMAScript 5, as distributed programs should not
    require exotic flags.
  - Our Node.js servers utilize some ECMAScript 6 features. Feel free to use
    those features in that environment, if the code is unlikely to be ported
    elsewhere.
- Place `'use strict';` at the top of functions.
- Don't be tricky.
- Do be explicit.

# Formatting

- Strip trailing whitespace.
- Use Unix linefeeds (LF).
- End files with a newline.

Most text editors can be configured to format whitespace. Please do this to
avoid whitespace-oriented commits and diffs.

Use JSCS to validate the format of your code. Your editor may offer JSCS
integration, which you may find useful if you are not familiar with the style
guide. You can also use our projects' Grunt or Gulp interfaces.

# English

## Spelling

Use real words for identifiers.

For instance, "element" is best written as "element", not "el", "elm", "elem",
or "e".

For your convenience, the following words also have a preferred spelling:

Word      | Best written as | Known to be written as
----------|-----------------|-----------------------
directory | directory       | dir
error     | error           | err, e
index     | index           | idx, i
request   | request         | req
response  | response        | res

Ubiquitous acronyms such as "http" and "url" are permissible.

## Capitalization

Capitalize only the first letter of words in identifiers.

For instance, a phrase such as "File Transfer URI", which contains a
normally-capitalized acronym, has the identifier `fileTransferUri` (as opposed
to something like `fileTransferURI`).

## Word choice

Prefer descriptiveness, then brevity.

When creating APIs with hooks for certain events, prefix names with "on". e.g.,
`onComplete`, `onFail`.

When talking about millisecond values since the Unix epoch, suffix names with
"time". e.g., `startTime = Date.now()`.

When naming predicate values or functions, prefix names with "is". e.g.,
`isReady`, `isAvailable()`.

# Syntax

## Variables

If ECMAScript 6 features are available, prefer `const`. Use `let` only when the
value needs to change.

Thanks to block scope and temporal dead zones, `const` and `let` variables need
not be defined at the top of a function, nor in single, comma-delimited
statements. In fact, they should be defined separately, and close to their site
of first use.

Otherwise, refer to Code Conventions.

## Functions

Use [function expressions][]. Do not use [function declarations][].

[function expressions]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function
[function declarations]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function

Do:

```js
var procedure = function () {
    // Function body...
};
```

Don't:

```js
function procedure() {
    // Function body...
}
```

Function expressions are more versatile than function declarations.

Like `var` statements, function declarations also "hoist" a variable in the
enclosing function's scope, which can result in errors not obvious to the
talented, casual observer. But unlike function declarations, `var` statements
can at least be consolidated at the beginning of functions in a self-enforcing
clarifying fashion.

In code that uses ECMAScript 6 features, we can instead bind functions with
`const` and avoid hoisting (and reassignment) altogether.

## Strings

Use single quotes (`'`) for string literals.

Use string concatenation. Do not use "multiline strings".

Do:

```js
var a = 'The quick brown fox ' +
        'jumps over the lazy dog';
```

Don't:

```js
var a = 'The quick brown fox \
jumps over the lazy dog.'
```

Multiline strings are harmful because a trailing space following the escaping
`\` will result in a syntax error invisible to the human eye.

# Quality

Use JSHint to validate the quality of your code and check for errors.

It is highly recommended that you integrate JSHint into your editor, or switch
to an editor that offers integration. Real-time error reporting can make you
more productive by pointing out syntactic and logical errors before runtime.

# Style

## Functionalism

Prefer `forEach`, `map`, `filter` and `reduce` over `for` loops.

Prefer `every` and `some` over `continue` and `break`.

Prefer iterating through own properties over `for in` loops.

Because tail calls are not yet widely implemented, use `while` and `do` for
algorithms that might grow the call stack boundlessly.

If Lodash is available, prefer its implementations of the aggregate functions
over the ones from `Array.prototype`. Also prefer utilities like `_.keys`,
`_.isArray`, etc, over the built-ins, as Lodash implementations sometimes fix
compatibility issues.

Aggregate operations provide forms for iterating over collections that are more
precise and oftentimes more useful than their procedural alternatives.

## Explicitness

### Existence

JavaScript has a concept of "truthiness" and "falsiness", where certain values
are coerced to boolean values to satisfy predicates. It makes sense that
`false`, `undefined` and `null` are falsy, but unfortunately, `''` and `0` also
fall under this category. Sometimes (perhaps more often than not) empty strings
and zero are "valid" values from a business perspective. Thus, when checking for
the "existence" of a value, it unreliable to rely on truthiness. Instead, you
should explicitly check for negatives.

Do:

```js
var number = value === undefined ? 100 : value;
if (number !== undefined) {
    // ...
}
```

Don't:

```js
var number = value || 100; // Excludes 0.
if (number) {
    // Won't execute if the value is 0.
}
```

### Simplicity

It's possible to do lots of things on one line. But that can make your code
confusing. You should try to do just one thing per line.

Do:

```js
value = array[index];
index += 1;
```

Don't:

```js
value = array[index++];
```

### Abstraction

It's possible to make implications. For instance, the value returned from
`indexOf` can be used to test for the existence of substrings or elements. But
it is better to abstract away implications so you can think about the actual
problem at hand. Therefore, a `contains` function would be better for checking
existence than `indexOf`.

Do:

```js
if (contains(array, 'value')) {
    // ...
}
if (isInteger(value)) {
    // ...
}
```

Don't:

```js
if (array.indexOf('value') > -1) {
    // ...
}
if (parseInt(value, 10) !== NaN) {
    // ...
}
```

# Credits

Influenced by:

- [Maintainable JavaScript](http://youtu.be/3MejbqcMC08)
- [Programming Style and Your Brain](http://youtu.be/_EANG8ZZbRs)
- [JavaScript - The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742)