![Wat](http://i.imgur.com/IppKJ.jpg)

The [ECMAScript 2018 specification](https://www.ecma-international.org/ecma-262/), for when you need to [dominate your enemies](https://rickandmorty.fandom.com/wiki/Raising_Gazorpazorp/Transcript).

- The [`in` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in), e.g. `"navigator" in window`, has been in JS since *IE 5.5*. It is related to `hasOwnProperty()`, except where `in` will literally tell you if the object has that property, whereas `hasOwnProperty` will only tell you if the object has that property, not inherited from a prototype.
- "JavaScript legend Douglas Crockford once said that [monads are cursed -- that once you understand monads for yourself you lose the ability to explain them to others](http://sean.voisen.org/blog/2013/10/intro-monads-maybe/)"
- `array.slice()` (with no arguments) gives you a quick shallow copy of an array.
- ["Nobody loves what prettier does to their syntax. Everyone loves what prettier does to their coworkers' syntax."](https://www.reddit.com/r/javascript/comments/8as6ns/i_dont_like_prettier/dx14ag5/)
- 'use strict' is unnecessary inside of modules. (ESLint)
- Prettier (v1.17.0) outputs [spaces inside object literals](https://prettier.io/playground/#N4Igxg9gdgLgprEAuEAzArlMMCW0AEqEEAFAJT7AA6U+hxJwAFnADasRL4DkA7hACdWAE24AaAEZcwAXzIBuGjJBiQEAA65oAZ2SgAhgIEReABUMJdKfQDcIOYSpASB+sAGs4MAMrq3OKABzZBgBdDhVJhgAW1YAdSYceG0-MDhvSyScGySAT2RwbV1VAO04ARhTV0Do-WRUfVYy1QArbQAPACFXDy9vfWi4ABkAuHrG5pA29u8AwNY4AEV0CHhxpoiQPwEygQKJfQlcjigndQEAmDiHGCZkAA4ABlVziDK413UC87hdmzHVABHFbwKoaKwgfTaAC0UDgcGECKcAjgwJwKKq+hqdSQDQ2qjK0RwITCm20cwWy1WY1xE02MEO12Et2QACZVKF9DhWHMAMIQaK1ApQaAAkDoMoAFUOVjxZRkMiAA) if the object is a one-liner. Also, ESLint looks for this.
- [`_.inRange`](https://github.com/lodash/lodash/issues/3147) is not inclusive on the end. `_.inRange(0.5, 0, 1)` checks `0 <= 0.5 < 1`, which is an interesting choice of inequalities.
- [Your workers cannot see your local storage.](https://dev.to/rdegges/please-stop-using-local-storage-1i04)
- [The event loop is very simple](https://www.youtube.com/watch?v=8aGhZQkoFbQ). If the _stack_ is empty and there is something in the _task queue_ (the stuff you put in with `setTimeout` or `setInterval`), then it puts the first task in the stack, and the stack runs your task.
- If you really want to see Jesus weep, you can force `getElementById` to return multiple elements by [changing the element's id, and then calling `getElementById`, rince and repeat, until no elements by that id can be found](https://stackoverflow.com/a/3607436/1558430).
- [Rollup](https://rollupjs.org/guide/en#why) the bundler uses the term _tree-shaking_ to refer to including only the code that is imported by your code.
- "You need to use yarn in flat mode with `yarn install --flat`"
- The codename for ionic v1 is [`hong kong`](https://github.com/ionic-team/ionic-v1/blob/master/package.json#L5).
- [JSX in excruciating depth](http://blog.klipse.tech/javascript/2016/12/14/jsx.html)
- `window.location.reload()` accepts either `true` or `false` for hard reload.
- `window.location.reload()` does not accept arguments across all browsers.
- The window's `storage` event is fired on [every window using the storage except the window that modified storage](http://stackoverflow.com/a/4689033).
- Delegated selectors save memory (most of the time): `$(parentObject).on(events, selector[, data], function (event) {})`
- `new Object`, or any object in general (e.g. `Date`), does not require `()` to initialise. Strict mode will throw a warning, though.
- It is possible to run a WebSocket inside a worker.
- A named closure (which is NOT an oxymoron: `(function abc() {... }())` has its function name scoped inside the closure.
- One use of the named closure (`(function abc() {}())`) is that `abc` is defined only inside the scope, which means recursive closures can be built without using an outside variable.
- Another use of named closures is if you want to return an object instantiator:

```
return function IndexError(msg) {
    // then do whatever you want with this 'class' outside the function
    // and all instances will at least have a name
};
```

- Detecting document zoom level [is a piece of ass](http://stackoverflow.com/questions/1713771/how-to-detect-page-zoom-level-in-all-modern-browsers).
- `Object.defineProperty` creates immutable object constants. [Everything except IE8](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FGlobal_Objects%2FObject%2FdefineProperty) does it correctly.
- In backbone.js, [a `View` is actually a controller](http://backbonejs.org/#FAQ-mvc).
- `return a, b` returns ~~whichever value is true first~~ the last value.
- [Scroll events do not bubble.](http://www.quirksmode.org/dom/events/scroll.html)
- There are [typed arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays/Int32Array).
- `inputElement.selectionStart` is not cross-browser compatible.
- As it turns out, there is a [documented way](http://en.wikipedia.org/wiki/JSON#JavaScript_eval.28.29) of validating JSON.
- `_.bindAll` binds for all future calls, too.
- Although it is always true that a script tag with both src and content will not execute its content, [it doesn't mean it is pretty to tell the src to execute its innerHTML](http://ejohn.org/blog/degrading-script-tags/).
- `new function () { return ... }` returns the return value instead of an object when the value is an object.
- [The simplest inheritance example out there](http://stackoverflow.com/a/1204386/1558430)
- [data uris are not worth pursuing](http://www.mobify.com/blog/css-sprites-vs-data-uris-which-is-faster-on-mobile/)
- There is a built-in [self](http://stackoverflow.com/questions/3309516/when-to-use-self-in-javascript) keyword, but usually pointing at `window`, it's not all that useful.
- A `setTimeout` with a delay of 0 calls the function when the call stack is empty.
- [The 3 snapshot technique](https://docs.google.com/a/willetinc.com/presentation/d/1wUVmf78gG-ra5aOxvTfYdiLkdGaR9OhXRnOlIcEmu2s/pub?start=false&loop=false&delayms=3000#slide=id.g31ec7af_0_58): First take a snapshot, then do something and take another snapshot. Repeat the exact same things and take snapshot 3. Finally, "filter objects allocated between snapshots 1 and 2 in snapshot 3's summary view"
- `_.once` keeps returning the value of the original call in subsequent calls.
- Terminology: `_gaq.push(['_trackEvent', 'category', 'action', 'label', 'value']);` [source](https://developers.google.com/analytics/devguides/collection/gajs/eventTrackerGuide#Anatomy)
- [Second parameter of `JSON.parse`](http://stackoverflow.com/questions/19281820/deserialization-of-partially-flattened-json/19281911?noredirect=1#19281911) (reverse applies to `.stringify`, too)
you will need to split the string first.
- [`debugger`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)
- `_.result(a)` returns `a()` if it is a function, or just `a`.
- `Number()` returns 1 or 0.
- If desperate, `_.omit(obj)` makes a copy of the object. `_.omit(obj, key1, key2, ...)` creates a copy without attributes key1 and key2.
- `navigator.onLine` is `true` when the browser is actually online, unless you are on a mobile device, in which case it is always `false`.
- Backbone: `Collection.parse` is called _only_ if you resolve the `fetch` XHR with remote JSON. It doesn't do anything if you resolve with an object.
- [Dispatching keyboard events without jQuery](http://stackoverflow.com/a/5920206/1558430)
- Clone an array: [`arr.slice(0)`](http://stackoverflow.com/questions/5024085/whats-the-point-of-slice0-here)
- [Marionette's UI hash](https://github.com/marionettejs/backbone.marionette/blob/master/docs/marionette.itemview.md#organizing-ui-elements) keeps references to UI elements; `this.ui.checkbox` anywhere in any method means `this.$(the checkbox selector`. This has no use for regions, whose elements are already defined using selectors.
- There's a [Promises 2.0](http://blogs.msdn.com/b/rbuckton/archive/2011/08/15/promise-js-2-0-promise-framework-for-javascript.aspx), but who uses it?
- > Note that the `bower_components` folder would normally be installed in the root folder but (angular-seed) changes this location through the `.bowerrc` file. Putting it in the app folder makes it easier to serve the files by a webserver.
- [Promises are streams](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754#request-and-response).
- Jinja2 and AngularJS template tags can conflict, which prevents Jinja2 from rendering the page. [Solution](http://zhangyelei.blogspot.ca/2013/10/variable-placeholder-conflict-between.html) is to override Jinja2's settings with some other tag: `JINJA_ENVIRONMENT=jinja2.Environment( loader=jinja2.FileSystemLoader(os.path.dirname(__file__)), extensions=['jinja2.ext.autoescape'], variable_start_string='((', variable_end_string='))', autoescape=True)` (I think it's a bit hacky, however)
- **NaCl** in tech usually stands for **Na**tive **Cl**ient.
- `"use strict";` in global scope affects the entire script file, but [not other scripts on the page](https://stackoverflow.com/questions/6483768/would-this-enable-use-strict-globally).
- To generate revisions of assets, you might need [gulp-rev](https://www.npmjs.org/package/gulp-rev) (see also: the "Works with gulp-rev" section)
- Use `bower` in place of npm for client side packaging to [avoid multiple versions of the same library sent to the client](http://stackoverflow.com/a/18652918).
- _Blocks_ (`{...}`) are mere syntax for command groups. The `if` statement executes anything after it, which is either a single command, or a block, which is a group of commands.
- `list[list.length] = 5` _is_ faster than `list.push(5)`, but [the gap is closing](http://jsperf.com/array-push-vs-index-push webtricksandtreats.com/javascript-array-push/)
- Assigning `someArray.length = 0` removes all items from the array. You can also assign other numbers, and if the array length goes from 0 to e.g. 3, the array is `[undefined, undefined, undefined]` (in the case of early nodes), or `[ , , ]` (in the case of modern nodes).
- `(new Date).getYear()` returns 115 for year 2015. To get 2015 instead of something useless, call `getFullyear()` instead.
- `+new Date` is the oldest trick in the book to get the current unix timestamp from JS.
- It is possible to give properties to a boolean, but because it has no prototype, it is impossible to read them again.

```
>var a = true
>a.foo = 'bar'
>a.foo
undefined
>true.prototype
undefined
```

- [Generators](http://www.2ality.com/2015/03/es6-generators.html) (good article) must be called `function*`s, with that ugly asterisk after the keyword. You call `.next()` on it until the return value's [`.done === true`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*).
- RequireJS: To provide fallback URLs for a library you reference, just [change its path to a list of paths](http://stackoverflow.com/a/12075285/1558430).
- The square brackets in `@param {type} [thing]` mean optional parameter in JSDoc.
- In addition to the public/private use distinction, ["a deferred (which generally extends Promise) can resolve itself, while a promise _might_ not be able to do so."](http://stackoverflow.com/a/6824836) (emphasis mine/yours)
- (and) _Futures_ are deprecated implementations of Promises.
- [`\S` is negated `\s`](http://stackoverflow.com/questions/4377480/what-does-this-s-regex-mean-in-javascript) (so anything but whitespaces)
- Adding a [label](https://www.reddit.com/r/javascript/comments/3cxkex/til_you_can_break_continue_to_a_label/) on top of a do/for/while loop allows `break` statements to specify how many levels to break, much like PHP's `break n`.
- [Setting any `window.onunload` handler](http://stackoverflow.com/questions/2638292/after-travelling-back-in-firefox-history-javascript-wont-run) forces javascript to be re-run when the page is loaded from a back button.
- Changing any part of `window.location` (e.g. `window.location.search = ''`), understandably, navigates away from the current location.
- MDN: ["When defining a variable that is meant to later hold an object, it is advisable to initialize the variable to null as opposed to anything else. That way, you can explicitly check for the value null to determine if the variable has been filled with an object reference at a later time."](http://stackoverflow.com/a/13143055/1558430)
- [Since Cordova 5.0.0](http://stackoverflow.com/a/30028686/1558430), [apps must declare `Content-Security-Policy`](https://github.com/apache/cordova-plugin-whitelist/blob/master/README.md) in order to go online.
- [HTTP PATCH](https://tools.ietf.org/html/rfc5789) is not meant to accept parts of an object; instead, it accepts a list of [JSON operation object](http://williamdurand.fr/2014/02/14/please-do-not-patch-like-an-idiot/) describing actions to be taken on the server, e.g. `PATCH /users/123 [{ "op": "replace", "path": "/email", "value": "new.email@example.org" }]` (replaces user 123's email to that.)
- If "bower" came from "[bowerbird](https://en.wikipedia.org/wiki/Bowerbird)", then bower is homophonous with "bauer", technically a German word.
- [`switch` is in fact coersion-safe](http://stackoverflow.com/questions/6989902/is-it-safe-to-assume-strict-comparison-in-a-javascript-switch-statement).
- `npm install linux` is [finally possible](http://hyperos.io/), thanks to HyperCore Linux.
- Lodash's `_.curry()` allows the use of itself as placeholders. `_.curry(func, _, 'foo')` curries `func` with the second argument specified.
- Assigning a property to `"a string"` and expecting it not to persist is [all well and good](http://stackoverflow.com/questions/5201138/why-cant-i-add-properties-to-a-string-object-in-javascript), but only AngularJS throws an Error when you do it. In other cases, the property simply reads `undefined`.
- You can `throw` anything. You can even `throw 'Hi';`. The argument given to the `catch` block is exactly what was thrown.
- Douglas Crockford [commits with awful messages](https://github.com/douglascrockford/JSLint/commits/master) (like you).
- `String(null)` works; `null.toString()` doesn't.
- [`includes()` rather than `contains()`](http://www.2ality.com/2015/11/tc39-process.html?m=1), because MooTools, apparently. [Incidentally, `[NaN, NaN].indexOf(NaN)` never finds any, and `includes()` does](https://stackoverflow.com/a/35370411/1558430), so use `includes()` whenever it is available, which is ES2016 and later.
- > [sessionStorage is just like localStorage, but it's local to the tab](https://github.com/mozilla/localforage/issues/2#issuecomment-27452423), so if you have two tabs on the same site they won't see each other's sessionStorage. [Opening a page in a new tab or window will cause a new session to be initiated.](https://developer.mozilla.org/en/docs/Web/API/Window/sessionStorage)
- TypeScript had the audacity to create a `void` type, which is `null ∪ undefined`.
- The modulo operator (`%`) does not do type checking. So, in an interesting way, one can write `"Hello %s" % "world"` in JS as if it does something, but in fact simply creating a `NaN`.
- JS Dates can be [invalid](http://stackoverflow.com/questions/1353684/detecting-an-invalid-date-date-instance-in-javascript). They are invalid if you add like a billion years to it, or the 30th of February, such that `isNaN(dateObj.getTime()) === true` (except in IE8, which [does not work](http://stackoverflow.com/questions/13878515/javascript-valid-date-checking-does-not-work-in-ie8-and-firefox) ).
- Calling [`url = URL.createObjectURL(blobOrFile)`](https://developer.mozilla.org/en/docs/Web/API/URL/createObjectURL) and opening it (either through `window.open` or some link-clicking means) will allow a file to be downloaded. Afterwards, remember to call `URL.revokeObjectURL(url)`
- No special case: `'yes' === 'yes' === 'yes' // false`
- `_.curry` returns a value only when all arguments are specified (so you can go `foo(1)(2)(3)` for a 3-argument function). `_.partial` is a single-layer wrapper.
- [Bower is dead](https://github.com/reactjs/redux/issues/944#issuecomment-154858804) because either bower fragments client/server libraries, or because people don't know how to resolve multiple-version dependencies with npm. I don't, for one.
- `fetch`ing with `mode: 'no-cors'` apparently [makes the request, but does not tell the client whether it succeeded or failed](http://stackoverflow.com/a/40182952/1558430).
- `bind()` is literally partialling, except maybe powerful. `console.log.bind(console, 'hello world').bind(console, 'eh')()` logs `hello world eh`.
- For some reason, [synchronous AJAX is allowed on window unload.](https://stackoverflow.com/questions/1821625/ajax-request-with-jquery-on-page-unload)
- [Try to be explicit](https://blog.scottnonnenberg.com/hard-won-lessons-five-years-with-node-js/) when it comes to imports. Finding usages of `foo.bar()` is much harder than `require('bar')`.
- [Web workers don't run if the procotol is file://.](https://stackoverflow.com/questions/21408510/chrome-cant-load-web-worker)
- If you `a = function () {}`, the function's name will be `a` (not sure why, but there you go.) If you `a = function b() {}`, the function's name will be `b`. If you `(function () {}).name`, that is an empty string. If you `(function (a) { console.log(a.name) }(function () {}))`, you will end up printing an empty string, but return `undefined`.
- `<!--` are intentionally allowed as comment markers in JS. [It is part of the spec.](https://github.com/denysdovhan/wtfjs#html-comments-are-valid-in-javascript) To get fired, you need to insert a space in between your mental gymnastics, like so: `if (5 < !--i) { ... }`.
- With that said, `<!--` is not valid comment syntax in ES6 modules.
- To `apply` a constructor, you [need](https://stackoverflow.com/questions/1606797/use-of-apply-with-new-operator-is-this-possible) to `new (Function.prototype.bind.apply(Foo, [null, a, b, c]));`
- `new RegExp(/already a regexp literal/)` can still be useful if you want to [add a flag to it](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/RegExp), like `new RegExp(/already a regexp literal/, 'i')`.
- `clobber: true` when using `fs.copy` or `copySync` would overwrite the destination file if it exists. The term [clobber](https://stackoverflow.com/a/9392784/1558430) might have come from `cp`, where `cp -n` has a man page saying "do not overwrite an existing file (overrides a previous -i option)".
- Use `Object.prototype.hasOwnProperty.call(obj, prop)` instead of `obj.hasOwnProperty`, [because](https://stackoverflow.com/a/12017703/1558430) anyone can define a `hasOwnProperty` on an object.
- [cordova-plugin-local-notifications](https://github.com/katzer/cordova-plugin-local-notifications) implements location-based notifications, but does not _support_ it, because everything is half-baked, and nothing is, strictly-speaking, well-documented.
- If you `var a = (1, 2)`, then `a` is just `2`, which makes sense once you realise there are no tuples.
- `console.error` in node does not show the traceback, and the colour is not red (not always, anyway). So the only difference between `console.error` and `console.log` is where it goes (stderr/stdout).
- ES 5.1 already has a method called [`Array.isArray()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray). "Little known fact: `Array.prototype` itself is an array"
- Simply leaving out the [fractional part](https://en.wikipedia.org/wiki/Fractional_part) of a number, i.e. `123.` instead of `123.0`, is completely valid symtax.
- In the line `var a = a || 5`, by the time the second `a` is accessed, `var a` has already run, so it will not throw ReferenceError.
- [`assert` is not a thing](https://stackoverflow.com/a/15313435/1558430) but you can write your own in, like, one minute.
- What people want from you when they ask you for ["design patterns"](https://github.com/fbeline/Design-Patterns-JS).
- [Client-side password hashing is so uncommon](https://security.stackexchange.com/a/143857) because "An active MITM can tamper with the JavaScript and disable hashing" (which would cause the password to be sent over plain text, becoming visible), or: "Client-side hashing is rare because people use SSL instead."
- Other people don't have the same problem as you ('wait for the device to be online before the app starts, so all the script tags in <head> are loaded') because their scripts were bundled with the app when built.
- A [gulp task that returns](https://stackoverflow.com/questions/21699146/gulp-js-task-return-on-src) is a synchronous gulp task. If your task A returns nothing and some other task B depends on task A, task A will simply be run and not awaited. So you should probably return something.
- The `{a, b, c} === {a: 'a', b: 'b', c: 'c'}` syntax is called [punning](https://reasonml.github.io/docs/en/record#syntax-shorthand).
- Adding [`)]}\n`](https://groups.google.com/forum/#!topic/repo-discuss/uzr84ZGI62g) in front of every JSON request is a way to protect against XSSI, or cross-site script inclusion. This makes the payload impossible to execute if included as a script tag, but trivial to remove the prefix if the data was retrieved using XHR.
- Use `nvm ls-remote` to find out what versions it has available to install.
- If the builder pattern allows the `.` in `.foo()` to be the first character for the line, then `,` can also be the first character in an object for the sake of having smaller diffs.
- Leaflet allows you to build any map, not just what's on earth. You can make [a map for Zelda](https://mrcheeze.github.io/botw-object-map/) if you wanted to.
- There are multiple event loops: the normal one, to handle `setTimeout` and such; and another one for [microtasks](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/), to handle promise resolution, apparently. Microtasks are processed before normal tasks.
- The [comma operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator) evaluates everything from left to right, and then returns the last item. But beware: in `a = (1, 2)`, `2` is assigned to `a` because `2` is the last item. However, if you do `a = 1, 2`, `a = 1` is evaluated first, so `a` is `1`.
- You can use `window.history.pushState()` to change the URL of the current browser window, but it is allowed only if the old and new URLs belong to the same domain, so you can't build a website that changes the URL to that of a banking website, for example.
- "[Wanting temporary storage (like `sessionStorage`) is a very common misconception: an application doesn't need that](https://github.com/cyrilletuzi/angular-async-local-storage/blob/main/docs/EXPIRATION.md#misconceptions) (unless) you need to share data between multiple tabs (...)  if your app is distributed as a website" - cyrilletuzi, front-end consultant, on why we don't need something (unless we need it, in which case we do).

## About JavaScript

- Officially-speaking, JS is weakly-typed. "Coercion is usually a symptom of weak typing."
- If [ECMAScript is the standard](https://codeburst.io/javascript-wtf-is-es6-es8-es-2017-ecmascript-dca859e4821c) and JavaScript is the most popular implementation of the standard, then you actually never _write_ ECMAScript.
- Perhaps JS doesn't support keyword arguments because they don't have room for that syntax: `foo(a=5)` is literally an assignment expression inside a function call, for which python must use `:=`.
- [According to Crockford, Netscape called it LiveScript, originally.](https://news.ycombinator.com/item?id=8344100) In their attempt to 'destroy Microsoft', they teamed up with Sun. One of Sun's original goals with Java was making it the client-side scripting language for the browser. However, Netscape had LiveScript. Apparently the negotiations almost broke down over this point. In an enlightened moment, (probably) Marc Andreessen proposed renaming LiveScript to JavaScript (despite the fact that the languages have very little in common), and joy was had. Sun got the JavaScript trademark (and passed it on to Oracle), and Netscape got a perpetual exclusive license to use it. When JavaScript was standardized to avoid Embrace, Extend, Extinguish, Netscape refused to share its license, and so the official language was renamed to ECMAScript, after the standards body. When Sun was bought by Oracle, it also got the trademark, and presumably, Mozilla inherited the exclusive license from Netscape.
- The `with` statement was banned [before it was even cool](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with).
- The TC in TC39 stands for [technical committee](https://www.ecma-international.org/memento/TCs&TGs.htm). [TC39](https://www.ecma-international.org/memento/TC39.htm) happens to be the committee that works on ECMAScript.

## Quirks and Features

- Large objects [that conform to the JSON spec] can actually improve performance if you [sent it as a string, and `JSON.parse()` it afterwards](https://v8.dev/blog/cost-of-javascript-2019), because the JSON spec is smaller than the JS spec, and can be parsed more efficiently.
- JavaScript is one of the few languages, [if not the only language](https://en.wikipedia.org/wiki/Undefined_variable#Examples), to have a concept of `undefined`, but whose result from `1/0` is `Infinity`, rather than `undefined`.
- JavaScript is perhaps the only language that has both `null` and `undefined`, but, on top of that, also has an uninitialised value known as the **Elision**, e.g. when you create a `new Array(10)`, [the array is `[ , , , , , , , , , ]` rather than `[ undefined , undefined, undefined, undefined, undefined, undefined, undefined, undefined, undefined, undefined ]`](https://stackoverflow.com/a/50326100/1558430), which you could get with `new Array(10).fill(undefined)`. *But wait, there's more*: `(new Array(10))[0] === undefined`! So there are two different kinds of undefined, one of which you cannot access, and have no control over.
- Whitespace in between symbols don't matter: `console . log ( 'lol' ) // lol`
- `NaN` always compares to false. Only `isNaN` can compare `NaN`.
- `~~` is a [fast Math.floor](http://stackoverflow.com/a/5971668/1558430), noting some differences about negative numbers ("in that it actually just removes anything to the right of the decimal. This makes a difference when used against a negative number. Also, it will always return a number, and will never give you NaN. If it can't be converted to a number, you'll get 0").
- [There are no leap seconds](http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.1).
- `void 0 === undefined`. Actually, `void anything === undefined`, and `void;` alone is not valid syntax.
- It was possible to define the global `undefined`; not possible anymore, because people were screwing around with it. It is still possible to declare your own `undefined` in any other scope.
- You can't send code to the server by POST and have the browser run the same code. ("Refused to execute a JavaScript script. Source code of script found within request.")
- On [why `null` is of type `object`](https://stackoverflow.com/questions/5076944/what-is-the-difference-between-null-and-undefined-in-javascript#comment9782995_5076962): "You may wonder why the `typeof` operator returns `'object'` for a value that is `null`. This was actually an error in the original JavaScript implementation that was then copied in ECMAScript. Today, it is rationalized that `null` is considered a placeholder for an object, even though, technically, it is a primitive value."
- `undefined == null` === `true`. Die in a fire, JS!
- [`null == 0` is false, `null > 0` is false, but `null >= 0` is true.](https://github.com/denysdovhan/wtfjs#comparing-null-to-0) The short explanation is: `==` will not convert `null` to a number, but `>` will (to 0). So `null` is not 0, which is true; `+null` is 0, which is not greater than 0; and `>=`, which is internally just `<`, converts the expression to `!(+null < 0)`, which is `!false`, aka `true`.
- `null >= null` and `null <= null` are both true (the effective comparison, after type coersion, is `0 >= 0`). Maybe they compare loosely. Ah but get this. `undefined >= undefined` and `undefined <= undefined` are both false, because the effective comparison is `NaN >= NaN`.
- Similar to the point above, while `undefined <= undefined` is false, `undefined < undefined || undefined == undefined` is obviously true, so you can't just replace all the `< || ==`s with `<=` as if they are equivalent.
- [Detecting `{}.__proto__`](http://foundation.zurb.com/docs/upgrading.html) is one of the fastest ways to tell if a browser is running on a [browser that also runs on mobiles](http://stackoverflow.com/a/3082878/1558430).
- `0.1 + 0.2 = 0.30000000000000004` (over). `0.7 + 0.2 = 0.8999999999999999` (under).
- `+almostAnything` converts it to an integer. (except objects, arrays, strings... so, almost nothing.)
- [`{} + {}`](http://stackoverflow.com/a/9033306/1558430), when run as-is, is the addition of an [empty block](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block) and an empty object. While two empty objects added together is `[object Object][object Object]`, because empty blocks are not empty objects (and it is entirely up to the interpreter and [the spec](http://www.ecma-international.org/ecma-262/5.1/#sec-12.1) to decide which one it is), the explicit construction of objects matters (`(a = {}) + {}` is adding two objects, for example)
- Because of the unique, abusive relationship you have with JS, [`string` does not autobox completely to `String`](https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip), and `typeof`ing the two yields different results, *and* `typeof String() !== typeof new String()`. Guess which is which.
- [`isNaN(...)` is not the same as `Number.isNaN(...)`](https://stackoverflow.com/questions/25176459/is-number-isnan-more-broken-than-isnan), where `Number.isNaN(undefined)` is false, but `isNaN(undefined)` is true. `Number.isNaN` will always return false if the input is not of the number type. In other words, [`Number.isNaN()` is probably the one you want](https://stackoverflow.com/a/25176688/1558430) if you are working on numbers.
- `isFinite(NaN)` is, quite contrary to all the other insanity that JS forces you to think is normal, actually returns `false`.
- Because [object keys are always `toString`ed](https://mathiasbynens.be/notes/javascript-properties), `{ .12e3: 'wut' }` can be retrieved using the key `120`.
- Unicode is allowed as variable names; emojis, being unicode, are not.
- Because JS is JS, `{b: undefined}` is perfectly valid, and defining `b` as `undefined` really defines `b`.
- Arrays can never be object keys. `{[] : 5}` is a syntax error. `{[foo] : 5}` is a reference error for not finding `foo` to unpack. `{['hello'] : 5}` is just `{'hello' : 5}`. Assigning an array i.e. `array[[]] = 5` will simply give it an empty string of 5 i.e. `{'' : 5}`.
- [Any bitwise shifting operator (e.g. `>>`) will convert its operands to 32-bit integers first](https://stackoverflow.com/a/12125452/1558430). For example, `123456789012345 >> 0` will first convert 123456789012345 (0000000000000000011100000100100010000110000011011101111101111001) to a 32-bit integer (10000110000011011101111101111001), which in two's complement ended up being -2045911175. (Note: >> 0 shifts the thing by exactly 0 bits, making it a pure JS-quirk operation.)
- JS is a (bitch-ass) weakly-typed language, so if you do `array[[1, 2]] = 3`, you can get 3 back using `array['1,2']`.
- Depending on which browser version you have, you can do these: `a = () => 'bar'` (ES6), `a = function() 'bar'` (some intermediate form that modern browsers no longer support).
- You know how you can't just pass `console.log` as a function? Use [`console.log.bind(console)`](http://stackoverflow.com/questions/6789689/javascript-abstract-console-logging) instead.
- [IndexedDB is hilariously slow](https://news.ycombinator.com/item?id=28997346), to the point where people (like the ones over at WatermelonDB) had to [use IndexedDB in memory](https://github.com/Nozbe/WatermelonDB/blob/master/docs-master/Implementation/Adapters.md#web) most of the time.
- In desperation, lodash introduces a concept of `nil` (with an [`_.isNil()`](https://lodash.com/docs/4.17.15#isNil) to check for it), which is (`null | undefined`).
- Borrowing so much from Java, JS uses `null` to denote "not an object". Because [a JS variable can hold both a primitive and an object](https://2ality.com/2013/05/history-undefined.html)---which Java's cannot---JS needed a value for "neither a primitive nor an object". `undefined` was born, [though a bit unnecessarily](https://twitter.com/BrendanEich/status/330775086208524288). And since this whole thing came from type coercion from `null` to `0` through weak typing, you can tell how much damage this initial decision did to the language.

### *Internet Explorer* Quirks and Features

- You also shouldn't blindly `in` everything; [`for (var i in window.external)`](http://andrew.hedges.name/experiments/in/) throws exceptions in IE.
- `function abc()` in IE8 or above are declared twice, due to a bug in [hoisting and initialisation](http://kangax.github.io/nfe/).
- Since the deprecation of IE6 and IE7, `window.elementFromPoint(clientX, clientY)` and `window.getClientRects() -> [t, l, r, b]` are available to you.
- [JSON on IE8? Nope](http://stackoverflow.com/a/4715399/1558430)
- AJAX on IE? Nope. Need to use `this.response || this.responseText` for any AJAX objects made.
- CORS is not supported before IE8; hence JSONP.

## Errors
- Throwing a non-string error simply coerces it to a string. `throw new Error(5.0)` prints `5`.
- `(An error).stack` gives you the stack.
- [The spec](http://stackoverflow.com/questions/13294658/throw-errormsg-vs-throw-new-errormsg) allows `throw Error()` as well as `throw new Error()`. The two are identical.

## jQuery

- [`$.type()`](http://api.jquery.com/jQuery.type/) will tell you what type something is, including... object vs array/regexp/date/error.
- jQuery 3 recommends this form of ready: `jQuery(function($) { ... })`, where `$` is the jquery object for sure.
- Delegated events can be triggered by `.trigger("myCustomEvent", [data])`.
- jQuery will ignore the `data` option when creating an element from string if the element already has data attribute(s).
- `$([...]).each` is faster when `for` loops when it contains elements, and slower when it contains an ordinary iterable.
- `$(":last")` does not require an element.
- `$.each(string)` [stopped working](http://stackoverflow.com/questions/20075938/jquery-each-to-iterate-over-a-string-in-newer-versions). Now - jQuery has a `$.fn.queue(function () { ... })` that is called whenever something gets dequeued, presumably because an operation is done.
- `$('#password').val()` works on Firefox's autocompleted password fields!
- [jQuery's `.append()`](http://api.jquery.com/append/) can accept plain HTML strings.
- `$.fn.not` is [**not** the opposite of `$.fn.is`](http://ajpiano.com/the-opposite-of-jquerys-is-method-is-not-not-it-is-is/) -- `$.fn.not` always gives you a truthy return.
- Apparently [\$.Deferred is a monad](http://sean.voisen.org/blog/2013/10/intro-monads-maybe/).

### Deferred API

- `$.when(<Deferred, Promise, or any object really>, another, another...) => <Promise>`
- `<Deferred>.then(done, fail, progress)`
- `<Deferred>.done(function, function, ...)`
- `<Deferred>.always(function, function, ...)`
- `<Deferred>.fail(function, function, ...)`

## [Promise API](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise)

- An executor is `function(resolve, reject) { ... }`
- A _Promise_ is `new Promise(executor);`
- `Promise.all([Promise, Promise, ...]) => <Promise>`
- If all promises in a `.all()` succeed, the call is resolved with an _array_ of their resolves.
- If any promise in a `.all()` fails, the entire call fails. The error in the `.catch()` is the error from the first failed call.
- `Promise.race([Promise, Promise, ...]) => <Promise>` aka `Promise.any` if it existed
- `Promise.resolve(value) => <Promise>`, `Promise.reject(value) => <Promise>` that are pre-resolved/pre-rejected
- `<Promise>.then(function succeed, function fail) => <Promise>` If any of these functions return anything, [the new promise will resolve/reject with their return values.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)
- `<Promise>.catch(function error)` If any of `(succeed, fail)` throws an Error, [the `function error` will receive the same Error.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)

## WebRTC

- Feeling evil? Great! WebRTC allows you to [get the user's internal IP](http://stackoverflow.com/questions/391979/how-to-get-clients-ip-address-using-javascript-only/32841164#32841164), track people without [having the network request show up on the network panel](http://www.imanevilblogger.net/tag/disable-webrtc/), take [pictures of heads](http://auduno.com/post/25125149521/head-tracking-with-webrtc) (?), and more!

## RxJS

- It's a [reactivex](http://reactivex.io/) library for JS, which implements the [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern). In the observer pattern, an object notifies subscribers of any changes.
- [RxJS's `tap()`](https://rxjs.dev/api/operators/tap) lets you put side effects in callbacks, instead of putting them in `map`.
- `Observable`s are so lazy, [they are not executed until `.subscribe()` is called on them](https://www.syncfusion.com/blogs/post/angular-promises-versus-observables.aspx).
- Observables come in two flavours: hot and cold. A cold observable produces data as soon as a `.subscribe()` is called (i.e. an observer is added). [A hot observable produces data even if no one is observing yet](https://stackoverflow.com/a/58692340/1558430), like a movie that you will miss parts of if you show up late. [Hot observables have producers outside; cold observables have their producers inside.](https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339)
- Use [`from(promise)`](https://angular.io/guide/rx-library#observable-creation-functions) to create an observable from a promise.
- An observable emits an update via [`observer.next(value)`](https://rxjs.dev/guide/observable#anatomy-of-an-observable).
- Once `observer.complete()` is called, any subsequent calls of `.next()` will not trigger an [observer's `next()`](https://rxjs.dev/guide/observer).
- An observer (a `observable.subscribe()` call returns an observer) can cancel a subscription by calling `observer.unsubscribe()`.
- "Adding an observer to another observer" (e.g. `observer.add(observer2)`) allows you to unsubscribe two subcriptions at the same time.
- A [`Subject`](https://rxjs.dev/guide/subject) is an observable that broadcasts values to many observers. This also means `Observable`s cannot be subscribed by multiple observers.

### Operators

- You use `.pipe()` to let each operator modify whatever's being observed. Subscribing to the empty pipe (`.pipe()`) will get you the same values as if you had not called the empty pipe.
- Piping to the `map((x) => ...)` function allows you to transform each value for the next operator in the pipe.
- Piping to the `scan((acc, val) => ...)` function allows you to accumulate values. For example, if you had an array of `[1,2,3,4]` and you had piped it through `scan((acc, val) => acc + val)`, the expected output is `[1, 3, 6, 10]`.
- Piping to the `filter(x => ...)` operator allows only items that meet your conditions to be emitted.
- Piping to the `tap()` function allows you to subscribe to a value in between pipe operators. For example, if you `.pipe(tap(...), map(...), tap(...))`, two different values are emitted to the observer: one before the map, and one after.

## [Writing memory-efficient JavaScript](http://www.smashingmagazine.com/2012/11/05/writing-fast-memory-efficient-javascript/)

- Dereferencing (`delete object.key`) does not free memory immediately, but it does take CPU to modify the object, so use sparingly.
- Setting something to `null` does not null the object; it merely sets the reference to `null`, which does not free memory immediately.
- Global objects are never garbage-collected.
- Function-scoped variables are cleaned up when it can no longer be reached (like, if it stays inside the function).
- Unbind event listeners when they're no longer used. This is not done automatically -- you need to keep track.
- Functions that return functions can never be garbage-collected, because the reference to the inner function can be called.
- (However,) any variables no longer used in the function that returns a function will be garbage collected in some cases (V8).
- Anything referenced by timers (`setTimeout` or `setInterval`) will not be garbage collected.
- Try-catches will cause V8 to cancel optimization.
- Don't write large functions; they are hard to optimize (both by humans and engines)
- To store many things of the same type (e.g. Number, String, ...) use an Array, because they are faster to iterate over [than objects with integer keys].
- Sparse arrays (most values are 0) are [slower on V8 than full arrays](http://jsperf.com/sparse-arrays-vs-full-arrays).
- [Except in Safari](http://jsperf.com/pre-allocated-arrays), never pre-allocate arrays.
- Avoid element reflowing/redrawing (but this is more of a DOM thing rather than JS)
- Trig (`sin`, `cos`) is [_MUCH slower_](http://jsperf.com/sin-cos-vs-sqrt) than `sqrt`, in cases where the former is applicable.
- Consider WebAssembly, which is "closer to binary". Files are also sent as binary rather than text, which is faster to load.
- A major con about VueJS was said to be that [they use the chat for everything](https://medium.com/@Pier/vue-js-the-good-the-meh-and-the-ugly-82800bbe6684) instead of documenting it, making it impossible for others to learn from what was already said.
- [Strict mode functions have `this` set to `undefined`](https://mathiasbynens.be/notes/globalthis)... except getters and setters. They have `this`.
- [`requestAnimationFrame` is never called when a tab is in the background.](https://developers.google.com/web/updates/2017/03/background_tabs)
- "[It turns out JavaScript engines are very good at iterating flat arrays and running small, highly optimized functions](https://news.ycombinator.com/item?id=14050625) and that's what Glimmer is doing at its core."
- [Trampolines](http://raganwald.com/2013/03/28/trampolines-in-javascript.html) are `while` wrappers that call an inner function for as long as the function remains a function, not a primitive value.
- ~~[No such thing as tail call optimization](http://stackoverflow.com/questions/3660577/are-any-javascript-engines-tail-call-optimized)... not one that works, anyway~~ES6 has tail call optimisation.
- To tail-call optimise your recursive functions, ensure your last return statement consists solely of [a single function call](https://hackernoon.com/es6-tail-call-optimization-43f545d2f68b) to the same function.

## Node

- [Shebangs are permitted in server-side `.js` files](http://stackoverflow.com/questions/10696222/how-to-make-javascript-support-shebang) run by nodejs or js.
- [The article about NodeJS 8 release](https://nodejs.org/en/blog/release/v8.0.0/), saying _'Note that, when referring to Node.js release versions, we have dropped the "v" in Node.js 8. Previous versions were commonly referred to as v0.10, v0.12, v4, v6, etc. In order to avoid confusion with V8, the underlying JavaScript engine, we've dropped the "v" and call it Node.js 8'_, was titled _'Node v8.0.0 (Current)'_.
- [Assigning anything to `process.env`](http://stackoverflow.com/questions/42170365/how-do-i-remove-a-value-in-process-env), even if it is `null` or `undefined`, converts it to their strings `"null"` and `"undefined"`. To delete a key from `process.env`, `delete` it.
- If you run `--save-exact` without `--save`, it doesn't save.
- Semver's `~` upgrades to any patch version. `^` upgrades to any minor version. npm stopped defaulting to the tilde because it assumes minor versions are all compatible with each other. In the real world (where npm is used), this is false.
- [Jeremy Ashkenas on semantic versioning](https://gist.github.com/jashkenas/cbd2b088e20279ae2c8e): _"SemVer tries to compress a huge amount of information — the nature of the change, the percentage of users that will be affected by the change, the severity of the change, into a single number." "SemVer is a false promise that appeals to many developers — the promise of pain-free, don't-have-to-think-about-it, updates to dependencies. But it simply isn't true." "It's alright for robots, but bad for us."_
- [npm 5 is still worse than yarn](http://blog.scottlogic.com/2017/06/06/does-npm5-deprecate-yarn.html)
- Node has its own "declaring which licence you use" syntax, called [SPDX](https://www.npmjs.com/package/spdx). `OR`, `AND`, and `WITH` are special keywords.
- The point of `devDependencies` is, _if you're making a server_, the deployment setup should exclude your test tools. If you write static web apps or otherwise run your setups from your local machine, this makes no difference.
- "Global" functions and/or variables in a node js file are only global for _that file_. If you want to make a function truly global, use `global.funcName = () => ...`.
- Adding `"bin": { "foo": "./cli.js" }` in your `package.json` adds a command called `foo` to your system when installed. Use [`npm link`](https://x-team.com/blog/a-guide-to-creating-a-nodejs-command/) to test it; use `npm unlink` to undo.
- Type `.load scriptName.js` in a node REPL to load it (note: shebangs are invalid.)
- You actually [can't](https://nodejs.org/api/esm.html#esm_enabling) `import anything from 'a module'` right now (2019).
- To spawn a process where you wish to keep your arguments together, like `"--foo bar"`, use the [`{shell: true}`](https://stackoverflow.com/a/52841103/1558430) spawn option.
- To debug a script, run `node inspect thatscript.js` instead of `node thatscript.js`.
- [The difference between `exports` and `module.exports`](https://stackoverflow.com/a/7142924/1558430) is, basically, you can't assign anything atomic to `exports`, i.e. `exports = foo...`. The _original_ reference to `exports` is exported, which means your new assignment is not.
- There is no difference between `npm install` and `npm i`. It is there because JS people type `npm install` so much.
- npm 5.2 gets [a new "npx" thing](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b) that is meant to execute one-off things (like `npx create-react-app blablabla`), running something with another version of node (`npx -p node@6 something`), or execute something installed in the relative path (`npx bower`, _`npx which bower`_, `npx mocha`) without needing that PATH line.
- If some npm package keeps complaining about unmet peer dependencies, even though you installed the dependencies first, [it might just need to be installed together, at the same time](https://github.com/palantir/tslint/issues/2647#issuecomment-298005316), like `npm install -g typescript tslint`.
- `npm ls` lists installed packages.
- `npm` has a [dedupe](https://www.npmjs.org/doc/cli/npm-dedupe.html) option that groups common dependencies higher up the dependency tree.
- node has a [debugger](http://nodejs.org/api/debugger.html). To use it, run `node debug` where you normally run `node`.
- "[Every function in Node.js is asynchronous](http://code.tutsplus.com/tutorials/node-js-for-beginners--net-26314)", even the ones that are normally blocking.
- `EADDRINUSE` is node's very polite way of saying "port is taken".
- [ALWAYS](http://blog.heroku.com/archives/2015/11/10/node-habits-2016) make an `.npmrc` that sets `save=true` and `save-exact=true`.
- [`n`](http://askubuntu.com/questions/426750/how-can-i-update-my-nodejs-to-the-latest-version) is the npm package that upgrades nodejs.
- [Don't rely on setInterval for long-running tasks.](https://github.com/nodejs/node/issues/22149) They stop working after 2^31 milliseconds.
- "Node JS" pronunciations [include](https://groups.google.com/forum/#!msg/nodejs/-d5LcWlQrxI/CarozdtVP3MJ) Node Jay Ess (commonly), No der Jay Ess, and most importantly, Node Dot Jizz.
- Placing `"scripts": { "postinstall": "bower install" }` in `package.json` (npm) installs `bower_components` right after npm finishes.
- For supporting the delusional "more choices is better" ideology, [npm allows its JS engine to be swapped out](http://www.marcusoft.net/2015/03/packagejson-and-engines-and-enginestrict.html) using either `engines` in `package.json`, or [an `.npmrc` file](http://blog.npmjs.org/post/110924823920/npm-weekly-5) that does the same.

### Webpack

- [Webpack](https://github.com/webpack/webpack) is browserify for AMD modules. (more accurately, it is the other way around.)
- The correct usage is `webpack your/index.js dist/bundle.js`. Just make sure you have one of those and you're good to go. Alternatively, make yourself a `webpack.config.js`, and run `webpack (--config webpack.config.js)` again. Also fine.
- "wdm" stands for [webpack dev middleware](https://github.com/webpack/webpack-dev-middleware).
- Tools like webpack and babel exist to solve browser compatibility problems that jQuery used to, says the people [here](https://news.ycombinator.com/item?id=21989967); they say "people ... should really stop complaining about 'complexity' in the web space ...".

### Testing

- Jasmine has both [`createSpy` and `createSpyObj`](http://stackoverflow.com/questions/24321307/what-is-the-difference-between-createspy-and-createspyobj). The former gives a function; the latter is an object with methods.
- [Karma is for testing client code only](http://stackoverflow.com/questions/16660670/how-to-test-nodejs-backend-code-with-karma-testacular),
- Replacing `it(...)` with `fit(...)` in a karma test suite will skip all tests except those marked with `fit`.
- `karma start --reporters dots,coverage` runs `karma` without the annoying long list of successes.
- Running just `karma start` without `--single-run` predictably runs karma whenever a file is changed.
