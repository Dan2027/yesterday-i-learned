# TypeScript

- Basic syntax: `function name(variable: type): type`

## Types

- Numbers: `number` (according to [the handbook](http://www.typescriptlang.org/Handbook), it is to be in lower case because `number` is the primitive type, while `Number` is the object type)
- Strings: `string`
- Typed arrays: `number[]`
- The `any` type literally means anything. It may well be omitted; the only use of it is to prevent errors when interacting with JS code.
- The `unknown` type is similar to the `any` type, except [something of `unknown` type cannot be assigned to a variable of some other type](https://stackoverflow.com/questions/51439843/unknown-vs-any), so the "any" type checking is not propagated.
- `void` is _only_ used to denote that a function returns nothing (`undefined` or `null`).
- `abcDef() as SomeType` [asserts the type of the result to a certain type](https://github.com/Microsoft/TypeScript-React-Starter/blob/master/README.md#type-assertions) when you know better than the static checker. TypeScript typically won't allow you to assert unless the function returns `any`, or has no definite return type, like [if a random function is involved](<http://www.typescriptlang.org/play/#src=function%20foo()%20%7B%0D%0A%20%20%20%20if%20(Math.random()%20%3E%200.5)%20%7B%0D%0A%20%20%20%20%20%20%20%20return%205%3B%0D%0A%20%20%20%20%7D%20else%20%7B%0D%0A%20%20%20%20%20%20%20%20return%20'5'%3B%0D%0A%20%20%20%20%7D%0D%0A%7D%0D%0A%0D%0Afunction%20bar(baz%3A%20string)%20%7B%0D%0A%20%20%20%20console.log(baz)%3B%0D%0A%7D%0D%0A%0D%0Abar(foo()%20as%20string)%3B>).
- [`??`, unlike `||`, falls back only for `null` or `undefined`, not for all falsy values.](https://startup-cto.net/10-bad-typescript-habits-to-break-this-year/) (edit: this is in JS proper now.)
- `a?.b` accesses `a.b` only if `a` is not null. `a[d]` is your standard JS. Since `?` is already a thing (it's a ternary operator, e.g. `a ? b : c`), and whitespace is nothing in JS, `a?[d]` doesn't work. You need to write `a ?. [d]` to access `a[d]` if `a` is not null.
- ...Or, `a?` can denote that `a` is nullable. So `a ? : string` is totally fine, but `a ? boolean : string` is totally not fine. Imagine that.
- The [`never` type](https://www.tutorialsteacher.com/typescript/typescript-never) is used to make sure nothing is ever assigned to it, including `null`, or to make sure a function never returns anything, including `null`. It is a bit like `: undefined`, but if you actually use `function foo(): undefined`, you are required to `return undefined`, which is a bit odd.

## Interfaces

### Class interfaces

Like in any other sane language, an interface is an abtract class with no code in its methods.

```
interface Foo {
    bar: number;
    setBar(baz: number);
}

class FooImpl implements Foo {
    setBar(baz: number) {  // Only constructor variables can be `private`
        this.bar = baz;
    }
}
```

`Foo` is never compiled to the resulting JS file. As far as the script is concerned, `Foo` never existed.

Classes can themselves have a "static" interface imposed on their constructors:

```
interface Foo {
    new (bar: number);
}
class Foo2 {
    constructor(bar: number) { }
}
var CheckedFoo2: Foo = Foo2;  // This may be a bit messy
var foo2Instance = new CheckedFoo2(1);
```

**Private methods cannot be declared in interfaces. Interfaces can only specify public methods.**

### Function interfaces

The thing that is "function interface" in TS controls what is required of an object that is passed into the function. Say you have

```
function foo(bar: object): number {
    return bar.baz;
}
```

It is implicit that the object `bar` must have an attribute `baz` that is a number. Therefore, it can be expressed as an interface

```
interface Bar {
    baz: number;
    optionalParameter?: boolean;  // If it is supplied, it must be a boolean
}
function foo(bar: Bar): number {
    return bar.baz;
}
```

Interfaces are good for catching typos.
It can also be used this way, on function expressions:

```
interface FuncSignature {
    (foo: string, bar: string): number;
}

// The function needs the same signature as its interface.
var func: FuncSignature = function (foo: string, bar: string): number {
    return 5;
}
```

`var func: FuncSignature` can be saved and used on multiple functions, so you might save some work by doing so.

**NOTE**: [Don't use the types `Number`, `String`, `Boolean`, or `Object`](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html) "These types refer to non-primitive boxed objects", aka you should just ask for the primitive types `number`, `string`, `boolean`, and `object`.

#### Function subtyping

From [the Handbook](http://www.typescriptlang.org/Handbook#type-compatibility-comparing-two-functions), a variable can be assigned a function, then be assigned another function with fewer [but the same types of remaining] parameters than it:

```
var x = (a: number) => 0;
var y = (b: number, s: string) => 0;

y = x; // "every parameter of x has a corresponding compatible parameter in y, so the assignment is allowed"
x = y; // "y has a required second parameter that x does not have, so the assignment is disallowed"
```

Incidentally, since the handbook talks about 'required parameters', this is valid:

```
var x = (a: number) => 0;
var y = (b: number, s?: string) => 0;  // Optional s

y = x; x = y;
```

### Array interfaces

If for some reason you need to make an interface for an already-typed array, you may:

```
interface StringArray {
    [index: number]: string;
}
var foo: StringArray = ['foo', 'bar'];
```

### Extending interfaces

Like classes, interfaces can be extended using the `extends` keyword.
If you use `extends` as a variable name in your existing JS files, you are an idiot.

### Abstract interfaces

Interfaces do not need to be used by any class before it is used. This example shows an interface `Foo` simply being used to check the ways the variable `c` is used. There is no `C` class.

```
interface Foo {
    bar: number;
}

var c: Foo;
c.bar = 5;
```

## Classes

Classes can have `private members: number;` and `static members: numbers`.
They can also have getters and setters, which downpiles to the same thing in ES6, for which [you may need a shim](http://kangax.github.io/compat-table/es5/#Object.defineProperty).

```
class Foo {
    private _bar: string;
    static kek: number;

    get bar(): string {
        console.log(Foo.kek);  // undefined (which is a number, obviously, since we declared it as such)
        return this._bar;
    }

    set bar(baz: string) {
        this._bar = baz;
    }
}
```

Unlike ES6 (before node v12's version of ES6 anyway), [it is valid syntax to set an instance's property directly inside the class](https://stackoverflow.com/a/35212197/1558430), outside of any method. The compiler appears to convert it to a constructor, with the `constructor` _at the end_ of everything else.

```
// TypeScript
class Foo {
    bar = 1;

    constructor() {
        this.bar = 2;
    }

    bar = 3;
}

// Transpiled code
var Foo = (function () {
    function Foo() {
        this.bar = 1;
        this.bar = 3;
        this.bar = 2;  // Notice the constructor body is now last
    }
    return Foo;
})();

// The effect is every (new Foo) has a bar of 2
```

Classes can be extended with multiple superclasses, aka. **Mixins**; but, for some reason, we use the keyword `implements` instead of `extends`.

> This treats the classes as interfaces, and only uses the types behind [the mixin classes] rather than the implementation.

### Decorators

[Decorators can all be on the same line](https://www.typescriptlang.org/docs/handbook/decorators.html#decorator-composition), i.e. `@f @g`

### Classes as interfaces

Classes with only static variables can be interfaces.

```
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {  // Magic
    z: number;
}
```

### Generics

A generic class is defined as such:

```
class Foo<T> {
    someMethodThatUsesT(bar: T) {
        alert(bar);
    }
}
```

And a generic class is instantiated as such:

```
var newInstance = new Foo<string>();  // you can now call someMethodThatUsesT with only strings.
```

## Performance

Microsoft tells you [how to write typescript with performance](https://github.com/microsoft/TypeScript/wiki/Performance).
