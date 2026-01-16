# Functions

A lot of the values provided in the default environment are
**functions**. A function is a piece of program wrapped in a value. Such
values can be _called_ in order to run the wrapped program.

Functions are one of the most central tools in programming. The concept
of wrapping a piece of program in a value has many uses. It gives us a
way to structure larger programs, to reduce repetition, to associate
names with subprograms, and to isolate these subprograms from each
other.

For example, the binding `prompt` holds a function asking users for
input:

```ts
const n = prompt("Pick a number");
```

Executing a function is called _invoking_, _calling_, or _applying_ it.
You can call a function by putting parentheses after an expression that
produces a function value. Usually you'll directly use the name of the
binding that holds the function. The values between the parentheses are
given to the program inside the function.

In the example above, the `prompt` function uses the string that we give
it as the text to show in the dialog box. Values given to functions are
called **arguments**. Different functions might need a different number
or different types of arguments.

## Return values

Showing a dialog box or writing text to the screen is a side effect.
Many functions are useful because of the side effects they produce.
Functions may also produce values, in which case they don't need to have
a side effect to be useful.

For example, the function `Math.max` takes any amount of number
arguments and gives back the greatest:

```ts
const greatest = Math.max(2, 4);
console.log(greatest); // => 4
```

When a function produces a value, it is said to _return_ that value.
Anything that produces a value is an expression in JavaScript, which
means that function calls can be used within larger expressions. In the
following code, a call to `Math.min` (which is the opposite of
`Math.max`) is used as part of a plus expression:

```ts
const sum = Math.min(2, 4) + 100;
console.log(sum); // => 102
```

Functions can be roughly divided into those that are called for their
side effects and those that are called for their return value (though it
is also possible to both have side effects and return a value).

A **pure function** is a specific kind of value-producing function that
not only has no side effects but also doesn't rely on side effects from
other code — for example, it doesn't read global bindings whose value
might change. A pure function has the pleasant property that, when
called with the same arguments, it always produces the same value (and
doesn't do anything else). A call to such a function can be substituted
by its return value without changing the meaning of the code. When you
are not sure that a pure function is working correctly, you can test it
by simply calling it and know that if it works in that context, it will
work in any context. Non-pure functions tend to require more scaffolding
to test.

Still, there's no need to feel bad when using functions that are not
pure. Side effects are often useful. There's no way to write a pure
version of `console.log`, for example, and `console.log` is good to
have. Some operations are also easier to express in an efficient way
when we use side effects.

## Defining functions

The most obvious application of functions is defining new vocabulary.
Creating new words in prose is usually bad style, but in programming, it
is indispensable.

Typical adult English speakers have some 20,000 words in their
vocabulary. Few programming languages come with 20,000 commands built
in, and the vocabulary that is available tends to be more precisely
defined, and thus less flexible, than in human language. Therefore, we
have to introduce new words to avoid excessive verbosity.

A function definition is a regular binding where the value of the
binding is a **function expression**. For example, this code defines
`square` to refer to a function that produces the square of a given
number `n`:

```ts
const square = function(n) {
    return n * n;
};

console.log(square(2)); // => 4
```

A function is created with an expression that starts with the keyword
`function`. Functions have a set of **parameters** (in this case, only
`n`), and a **body** which contains the statements that are to be
executed when the function is called. The body of a function created
this way must always be wrapped in braces, even when it consists of only
a single statement.

A `return` statement determines the value the function returns. When
control comes across such a statement, it immediately jumps out of the
current function and gives the returned value to the code that called
the function. A function without a `return` statement or a `return`
keyword without an expression after it will cause the function to return
`undefined`.

It is important to distinguish a _function expression_ from its _return
value_. The value of `square`, for example, is a _function expression_,
whereas the _return value_ of the function is the result of `n * n`.

```ts
console.log(square); // => [Function: square] (function expression)
console.log(square(2)); // => 4 (return value)
```

Assigning a function expression to a variable clearly demonstrates that
functions are values, but you should not define functions this way.
Instead, we will use **function declarations**.

### Declaration notation

When the `function` keyword is used at the start of a statement, it
works differently:

```ts
function square(n) {
    return n * n;
}
```

This is a **function declaration**. The statement defines the binding
`square` and points it at the given function. It is easier to read
because the first word of the statement clearly indicates that it
defines a function.

There is one subtlety with this form of function definition:

```ts
console.log(future()); // => "You'll never have flying cars"

function future() {
    return "You'll never have flying cars";
}
```

The preceding code works, even though the function is defined _after_
the code that uses it. Function declarations are not part of the regular
top-to-bottom flow of control. They are conceptually moved to the top of
their scope and can be used by all the code in that scope. This is
sometimes useful because it offers the freedom to order code in a way
that seems the clearest, without worrying about having to define all
functions before they are used.

### Arrow functions

There's a third notation for functions, which looks very different from
the others. Instead of the `function` keyword, it uses an arrow (`=>`)
made up of an equal sign and a greater-than character:

```ts
const roundTo = (n, step) => {
    const remainder = n % step;
    return n - remainder + (remainder < step / 2 ? 0 : step);
};
```

The arrow comes after the list of parameters and is followed by the
function's body. If the body is a single expression rather than a block
in braces, that expression will be returned from the function. These two
definitions of `square` do the same thing:

```ts
const square1 = (n) => {
    return n * n;
};
const square2 = (n) => n * n;
```

There's no deep reason to have both arrow functions and function
expressions in the language. Apart from minor details, they do the same
thing. Arrows functions make it easier to write small throwaway
functions in a less verbose way. We will use them often as arguments to
higher-order functions, which we will discuss later. Otherwise, prefer
function declarations as they are easier to read.

### Parameters

A function can have multiple parameters or no parameters at all. In the
following example, `makeNoise` does not list any parameter names:

```ts
function makeNoise() {
    console.log("Pling!");
}
```

Whereas `roundTo`, which rounds `n` to the nearest multiple of `step`,
lists two:

```ts
function roundTo(n, step) {
    const remainder = n % step;
    return n - remainder + (remainder < step / 2 ? 0 : step);
}
```

Parameters to a function behave like regular variables, but their
initial values are given by the caller of the function, not the code in
the function itself.

> [!NOTE]\
> What is the difference between a **parameter** and an **argument**?
> When _defining_ a function, you list its parameters. When _calling_ a
> function, you pass arguments.

## Scope

Bindings have a **scope**, which is the part of the program in which the
binding is visible. For bindings defined outside of any function or
block, the scope is the whole program — you can refer to such bindings
wherever you want. These are called **global bindings**.

Bindings created for function parameters or declared inside a function
can be referenced only in that function, so they are known as **local
bindings**. Every time the function is called, new instances of these
bindings are created. This provides some isolation between functions —
each function call acts in its own little world (its local environment)
and can often be understood without knowing a lot about what's going on
in the global environment.

Each scope can "look out" into the scope around it. The exception is
when multiple bindings have the same name — in that case, code can see
only the innermost one.

## Documentation

Functions are often meant to be used by others, and so it is crucial to
document their use. Today, most JavaScript programs use [JSDoc] and
[TypeScript] to do so. Both are _extensions_ to JavaScript; they are not
part of the official language specification.

### JSDoc

JSDoc lets you document your source code by adding a special comment
starting with `/**` directly above the function you want to document.
For example, here's a JSDoc comment describing what `roundTo` does:

```ts
/** Round n to the nearest multiple of step. */
function roundTo(n, step) {
    const remainder = n % step;
    return n - remainder + (remainder < step / 2 ? 0 : step);
}
```

Special JSDoc tags can also be used to give more information, such as
the type of parameters and return values, but in this course TypeScript
will be used for that purpose instead.

### TypeScript

TypeScript is a _superset_ of JavaScript that adds static typing and
type annotations. In TypeScript, a function's parameters and return
value have a type, which must be specified in an annotation. These
**type annotations** define a **contract** that must be followed when
using the function.

The updated version of `roundTo` declared below, for instance, must
always be applied on two numbers, and must always return a number. You
will get an error if that is not the case.

```ts
/** Round n to the nearest multiple of step. */
function roundTo(n: number, step: number): number {
    const remainder = n % step;
    return n - remainder + (remainder < step / 2 ? 0 : step);
}
```

You can use any of the primitives in a type annotation: `number`,
`string`, `boolean`, etc. TypeScript also includes more complex types
that we will cover later.

Here is another example :

```ts
/** Greet the person with the given name. */
function greet(name: string): void {
    console.log(`Hi, ${name}!`);
}
```

The contract of the `greet` function indicates that it must be applied
on a `string`, and that it returns `void`. In TypeScript, a function
without a return value has the return type `void`.

Another type which you might come across is `any`, which stands for
_any_ type. TypeScript effectively treats `any` as "please turn off type
checking for this thing". Although this might seem like an easy solution
when debugging, it should be avoided. Before writing the body of a
function, you should clearly define the types of values it accepts and
the type of value it returns.

[JSDoc]: https://jsdoc.app/
[TypeScript]: https://www.typescriptlang.org/

### Examples

Unfortunately, JavaScript does not provide a built-in way to record
examples or write automated tests. Instead, external libraries such as
[Jest] or [Vitest] are commonly used. Fortunately, these libraries offer
a similar interface through functions like `expect` and `toBe`.

For example:

```ts
/** Round n to the nearest multiple of step. */
function roundTo(n: number, step: number): number {
    const remainder = n % step;
    return n - remainder + (remainder < step / 2 ? 0 : step);
}

expect(roundTo(16, 5)).toBe(15);
expect(roundTo(14, 10)).toBe(10);
```

This code indicates that we expect the value of `roundTo(16, 5)` to be
`15`, and the value of `roundTo(14, 10)` to be `10`. If this is not the
case, an error will be displayed.

You can use these functions when running a JavaScript program with Bun
by including the following statement at the top of your file:

```ts
import { expect } from "bun:test";
```

[Jest]: https://jestjs.io/
[Vitest]: https://vitest.dev/
