- ES6 was the last big ECMAScript release. After that, they decided to make a new release every year, starting from the year 2016.

## `for`

- [`for a in b`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) loops over indexes in an array, or keys in an object. Anyway Mozilla says [there's no point in using `for...in` apart from debugging purposes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in#Why_Use_for...in); use `[].forEach` or `for...of` instead.
- [`for a of b`](https://alligator.io/js/for-of-for-in-loops/) loops over values in an array, or throws an error if you use it on an object (because it is "not iterable", it says. But didn't you just iterate through one using `for..in`?).
- Fun fact: [`of` is not a keyword, but `in` is](https://stackoverflow.com/questions/63731109/why-of-is-not-javascript-keyword), even after the introduction of this `for a of b` functionality.

## [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) and `const`

- `let` declarations are not hoisted.
- If there's a `var` in an `if` block, it will be declared outside the block [to the nearest function scope](http://ariya.ofilabs.com/2013/05/es6-and-block-scope.html). `let` and `const` limit their scope to inside the block (which, if you like brackets, makes some sense)
- `let`s cannot be declared in the same scope twice. So, you cannot use `let`s in multiple `switch` statements.
  - You can use `for(let i = 0;...)` to limit `i` to the `for` block, and jshint won't complain like it does for `var`.
  - ~~You can also use `let` to directly make a block: `let(foo='bar', baz='buz') { /* use foo inside */}`~~ I don't think it works anymore
- `const` [does NOT](http://exploringjs.com/es6/ch_variables.html) mean immutable. If you `const` an object, it is still mutable; the const itself just cannot be reassigned.
- If you manage to `const something = somethingThatThrowsAnError()`, then a) you get an error, b) `something` is not defined (not `undefined`), and c) you can't `const something` ever again.
- If you actually want to make an immutable object, you cannot. But there is [`Object.freeze`](https://mathiasbynens.be/notes/es6-const), which is shallow.

## Arrays and array-like things

- [Array comprehension](http://ariya.ofilabs.com/2013/02/es6-and-destructuring-assignment.html):

```
[for (x of ANY_ARRAY) for (y of ANY_ARRAY) for (z of ANY_ARRAY) (x + y + z)];  // unlimited number of `for`s

// these two are the same in what they do:
[console.log(t, a) for ({title: t, author: a} of books)];
[for (book of books) console.log(book.title, book.author)];
```

- `[,].length` is 1. `[1,].length` is also 1. `[1,,].length`, which is new, is 2. For any array with [dangling commas](https://davidwalsh.name/es7-es8-features) (and only dangling commas), the length is equal to the number of commas. If not (for example, `[1,,,1].length` is 4), the length is increased by 1.
- Also fun fact: the items stored in between dangling commas is `<x empty slots>` rather than `undefined`.
- The collection of new syntax also leads to new functions like this one: `const createArray = (length) => [...Array(length)];`.

### New methods

- The equivalent to `all()` is [`.every()`](https://zabanaa.github.io/notes/functional-programming-and-javascript-arrays.html): `let bar: boolean = array.every(x => isTrue(x))`
- The equivalent to `any()` is [`.some()`](https://zabanaa.github.io/notes/functional-programming-and-javascript-arrays.html): `let bar: boolean = array.some(x => isTrue(x))`
- [Use `.includes()` instead of `.indexOf()` whenever appropriate](https://dev.to/adroitcoder/includes-vs-indexof-in-javascript) because `.indexOf()` suffers from edge cases, such as `NaN`, `undefined`, `+0 / -0`, where they cannot be found even though they are in the array.

## Destructuring

- Destructuring assignment:

```
var [m, d, y] = [3, 14, 1977];  // need to wrap both in array notation
var [,,y] = [3, 14, 1977];  // can ignore variables you don't need
var [a] = [];  // "Fail-soft": a is undefined rather than causing an error
var [b = 1] = [];  // Defaults: b is 1 rather than undefined
```

[Destructuring objects](https://github.com/DrkSephy/es6-cheatsheet#destructuring-objects)

```
let luke = { occupation: 'jedi', father: 'anakin' }
let {occupation, father} = luke;
console.log(occupation); // 'jedi'
console.log(father); // 'anakin'
```

Such a destructure can be nested:

```
let {a: {b: c}} = {a: {b: 5}};  // c is now 5
```

It can also be done with brackets (`let (a, b, c) = {'a': ..., ...}`).
Do note that this can have a special case with just one unpack:

```
const { foo } = {foo: 'bar'};  // foo is bar because you unpacked foo from the object
```

AND [the special case where you unpack to nothing](http://www.2ality.com/2015/01/es6-destructuring.html):

```
const {} = 'abc';  // "OK, strings are coercible to objects"
const {} = undefined;  // Error
```

Function signatures are no special case. They also automatically destructure an object:

```
function getFullName({ firstName, lastName })  // Extracts firstName and lastName from the first argument object
```

## Functions

- Spread (packing) and Rest (unpacking):

```
function foo(a, b, ...rest) { /* rest is an array */ }
foo(...[1,2,3,4,5]);
```

(The `let { a } = {a: 2}` unpacking syntax is also called spreading.)

- Arrow notation: only `=>`, no `->`, and no context binding.
- An arrow function that has only one argument should not have parens, i.e. `.then(foo => foo.bar)`, but it so happens that you cannot have an arrow function like that, that also accepts an argument called `async`, i.e. `.then(async => async.bar) // won't work`. What you can do is: `.then((async) => async.bar)`. Either way, if you merge something like that, I will kill you.
- You can't unpack in a return statement because that's what square brackets are for: `(() => ({a: 4}))(){a} // won't work`

```
array.map(p => p * 2);
array.reduce((p, q) => (p + q));  // example with two arguments
```

[`arguments` cannot be used inside arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#No_binding_of_arguments).
`this` can be used in an arrow function, and [it is the `this` of the scope containing it](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#No_separate_this). rather than the usual craziness that comes with `bind`ing this and that and `self` and stuff.

- [Arrow functions can be multiline](http://ilikekillnerds.com/2015/01/a-guide-to-es6-arrow-functions/), but they also make the `return` statement compulsary (if you want to return something).
- Note: [arrow functions (basically) cannot have names](https://stackoverflow.com/questions/27977525/how-do-i-write-a-named-arrow-function-in-es2015).

```
var a = (foo) => {
    return foo;
};

a(3)  // 3
```

- [Default arguments](http://ariya.ofilabs.com/2013/02/es6-and-default-argument.html):

```
function foo(bar=5) {
    console.log(bar);
}
```

- Keyword unpacking: given an object as the argument, using a ["set notation" from CoffeeScript](https://github.com/jashkenas/coffeescript/issues/2427) unpacks them into the scope:

```
function foo({x, y=5}) {  // note: object notation (y:5) is not valid syntax at this particular location
    console.log(x);  // 4
}
foo({x: 4, y: 5})
```

- [Function in object](https://strongloop.com/strongblog/javascript-es6-object-notation/) shorthands:

```
{
  hello() {
    // ...
  },
  'hello world'() {
    // ...
  }
}
```

is equivalent to

```
{
  hello: function () {
    // ...
  },
  'hello world': function () {
    // ...
  }
}
```

and a clutch to adding dynamic names is, for some reason, done with square brackets:

```
let foo = 'foo'
const obj = {
  [foo + 'bar']: true
}
```

## Classes

- Classes are not hoisted, even if they down-transpile to a function. The reference is at the top of the scope, but the anonymous function is assigned to the reference on the line the class is declared.
- [Classes can be anonymous.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes#Class_expressions)
- [Class definitions are block-scoped, and cannot be redeclared in the same scope.](https://stackoverflow.com/a/36420130/1558430)
- If you have the balls to have a [class extends `null`](https://github.com/denysdovhan/wtfjs#function-is-not-function), be prepared to see unexpected behaviours ("function is not a function").
- Neither `a = 5` nor `a: 5` is/are valid syntax directly inside a class... [until ES2017 ish or Node 12](https://medium.com/@charpeni/arrow-functions-in-class-properties-might-not-be-as-great-as-we-think-3b3551c440b1). Transcompilers like babel will transpile that as `constructor() {this.a = 5}`, similar to what TypeScript does.
- [Class property methods are attached by the instance constructor](https://medium.com/@charpeni/arrow-functions-in-class-properties-might-not-be-as-great-as-we-think-3b3551c440b1), so [a subclass with an overridden property method with the same name cannot use `super` to access it](scripts/class-property-inheritance.js). It is also 20x slower to run methods like that, vs normal class methods (see article).
- You call a superclass's constructor method using `super()`, but you can only call a superclass's constructor like that in `constructor`s. You also cannot call a constructor by name, i.e. `SomeClass.constructor()`, or `new SomeClass.constructor()`. [See source](sources/0004.js). In other words, you can only use `super()` for `constructor`, and you can only use `super.foo()` for other methods.
- Classes can have [`#privateStaticMembers`](https://www.sitepoint.com/javascript-private-class-fields/). You haven't used it because it is supposed to come out in ES2019. Accessing a `#privateMember` gives you `SyntaxError` instead of the more common `TypeError` (for when something is supposedly undefined).
- Because of all being syntactic sugar, [a class can have two methods in it with the exact same name](https://eslint.org/docs/rules/no-dupe-class-members) (e.g. `class A { b() {} b() {}}`). The last `b() {}` becomes the active definition.

## [Proxies](http://ariya.ofilabs.com/2013/07/es6-and-proxy.html)

Proxies: a virtual wrapper that can handle property reads and changes on the original object.

```
var engineer = { name: 'Joe Sixpack', salary: 50 };

var interceptor = {
  get: function (target, name) {
    // https://www.atyantik.com/proxy-javascript-es6-feature/
    return name in target ? target[name] : 42;  // all missing keys default to 42
  },
  set: function (receiver, property, value) {
    console.log(property, 'is changed to', value);
    receiver[property] = value;
  }
};

engineer = Proxy(engineer, interceptor);
```

- Getter and setters in object shorthands:

```
{
  get hello() { ... }
  set hello() { ... }
}
```

was previously available with an awkward syntax in ES5.

* If a getter `length` returns `0`, then `foo.length === 0` will always be true because the getter is evaluated first.

- Generators require that ugly asterisk after `function`:

```
   // The asterisk after `function` means that
   // `objectEntries` is a generator
   function* objectEntries(obj) {
        let propKeys = Reflect.ownKeys(obj);

        for (let propKey of propKeys) {
            // `yield` returns a value and then pauses
            // the generator. Later, execution continues
            // where it was previously paused.
            yield [propKey, obj[propKey]];
        }
    }

    for (let [key,value] of objectEntries(jane)) {
        console.log(`${key}: ${value}`);
    }
```

## Imports

Imports and [default imports](http://www.2ality.com/2014/09/es6-modules-final.html) are similar, the only difference being whether the import is wrapped in braces.

```
export default function () { return 4 }
export function foo() { return 5 }
// ---
import CallItAnything, {foo} from 'that_file';
CallItAnything();  // 4
foo();  // 5
```

[Default imports can be mixed with named imports on a single line.](http://stackoverflow.com/a/31098182/1558430)

```
export default function () { .. this is React }
export function Component () { ... }
export function PropTypes () { ... }
---
import React, { Component, PropTypes } from 'react';
===
import React from 'react';
import { Component, PropTypes } from 'react';
===
Importing the default export under the name "React"
Importing the two named exports under the same names
```

- You can also `import * from 'a library'`.
- You [cannot](http://stackoverflow.com/questions/30340005/importing-modules-using-es6-syntax-and-dynamic-path) import dynamic paths. (In python, you `__import__(dynamic)`.)
- `require('a library')` is slower than imports, as the latter can be optimised statically.
- `import` is [not](http://adrianmejia.com/blog/2016/08/12/Getting-started-with-Node-js-modules-require-exports-imports-npm-and-beyond/#Imports) available in node 6.
- Use [the `module` script type](https://developers.google.com/web/fundamentals/primers/modules), i.e. `<script type="module" src="foo.ejs">`, and you can import things in your script.
- [Modules are evaluated only once](https://developers.google.com/web/fundamentals/primers/modules), while classic scripts are evaluated however many times you add them to the DOM.
- Due to the unique way JavaScript is funded, `import` is a hard keyword, but `from` and `of` are not, despite clearly being used in constructs `import ... from ...` and `for ... of ...`.

## Async/Await (ES7)

- `async function`s always return a promise. A `return 5` in an async function returns a promise that resolves with 5.
- `async function`s always reject with the error if an error is thrown in it.
- `await asyncFunction()` always directly returns the value that the async function resolves.

[Only in an `async`-marked function can you use `await`](http://masnun.com/2015/11/11/using-es7-asyncawait-today-with-babel.html). An `await` in a non-`async function` throws a SyntaxError.
If a promise is resolved, then the lines after `await` run. Otherwise, it throws an error and any `catch` blocks run.
If an async function has multiple return points: since a promise can only resolve once, it will always resolve with the first value.

- If you can't use `await` because you aren't in a function (for example, you're in a main script), then [wrap your script](https://stackoverflow.com/a/46515787/1558430) in `(async () => { ... })();` lol. (A future node version may allow top-level await.)

## WeakMap

WeakMap allows [garbage collection](https://stackoverflow.com/questions/29413222/what-are-the-actual-uses-of-es6-weakmap) of unimportant things.

## ES 2017

- RegExp will now support negative lookahead, which uses `(?<!foo)` to ensure a pattern is not preceded by another pattern. `(?<!foo)bar` will never match `foobar`.

## [ES 2019](https://blog.logrocket.com/new-es2019-javascript-features-every-developer-should-be-excited-about/)

- `catch` no longer requires you to say `(error)` if you don't use it.
- The "pipeline" operator (`|>`) allows you to write `a = bar(foo(a))` as `a = a |> foo |> bar`, which is sometimes easier to read. They [say](https://github.com/tc39/proposal-pipeline-operator) the operator came from F#, not because both `|` and `>` are already taken.

## Strings

- [Functions can be called without parentheses if the argument is a template string](https://michelenasti.com/2018/09/19/Javascript-chiamare-funzioni-senza-usare-parentesi-%28what!%29.html). This is called a [tagged template](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals). ` hello ``Michele``` calls `hello`with`Michele`sure, but`hello `Michele`, ``foo``` calls `hello` with `foo`, so it is not readable.
- There is an [string.endsWith](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith) now. Otherwise, the way to do it is checking if the substring of the larger string is exactly your search string, i.e. `haystack.substring(haystack.length - needle.length, haystack.length) === needle`.
