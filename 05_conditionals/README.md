# Conditionals

When your program contains more than one statement, the statements are
executed as though they were a story, from top to bottom. For example,
the following program has three statements. The first asks the user for
a number. The second, which is executed after the first, assigns a name
to the square of that number. The third, which is executed after the
second, prints a message.

```ts
const n = Number(prompt("Pick a number"));
const sqrt = n * n;
console.log("Your number is the square root of " + sqrt);
```

> [!NOTE]\
> The function `Number` [coerce] a value to a number. It is needed here
> because the result of `prompt` is a _string_ value, and we want a
> _number_. There are similar functions called `String` and `Boolean`
> that convert values to those types.

Not all programs are straight roads. We may, for example, want to create
a branching road where the program takes the proper branch based on the
situation at hand. This is called **conditional execution**.

In JavaScript, conditional execution is created with the `if` keyword.
In the simple case, we want some code to be executed if, and only if, a
certain condition holds. We might, for example, want to show the square
of the input only if the input is actually a number:

```ts
const n = Number(prompt("Pick a number"));
if (!Number.isNan(n)) {
    const sqrt = n * n;
    console.log("Your number is the square root of " + sqrt);
}
```

With this modification, if you enter "parrot", no output is shown.

> [!NOTE]\
> The `Number.isNaN` function is a standard JavaScript function that
> returns `true` only if the argument it is given is `NaN`
> (not-a-number). The `Number` function happens to return `NaN` when you
> give it a string that doesn't represent a valid number. Thus, the
> condition translates to "unless n is not-a-number, do this".

The `if` keyword executes or skips a statement depending on the value of
a boolean expression. The deciding expression is written after the
keyword, between parentheses, followed by the statement to execute.

```
if (<condition>) <statement>
```

The statement may or may not wrapped in braces (`{` and `}`). The braces
can be used to group any number of statements into a single statement,
called a **block**.

```
if (<condition>) {
    <statement>
    <statement>
    <statement>
}
```

You often won't just have code that executes when a condition holds
true, but also code that handles the other case. You can use the `else`
keyword, together with `if`, to create two separate, alternative
execution paths:

```ts
const n = Number(prompt("Pick a number"));
if (!Number.isNan(n)) {
    const sqrt = n * n;
    console.log("Your number is the square root of " + sqrt);
} else {
    console.log("Hey. Why didn't you give me a number?");
}
```

If you have more than two paths to choose from, you can "chain" multiple
`if`/`else` pairs together. Here's an example:

```ts
const n = Number(prompt("Pick a number"));

if (num < 10) {
    console.log("Small");
} else if (num < 100) {
    console.log("Medium");
} else {
    console.log("Large");
}
```

The program will first check whether `n` is less than 10. If it is, it
chooses that branch, shows `"Small"`, and is done. If it isn't, it takes
the else branch, which itself contains a second `if`. If the second
condition (`< 100`) holds, that means the number is at least 10 but
below 100, and `"Medium"` is shown. If it doesn't, the second and last
`else` branch is chosen.

[coerce]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#number_coercion

## Narrowing

The `prompt` function returns a value whose type is formed from two
other types: `string` and `null`. Since it depends on user input, it's
impossible to determine which one of these two types the function
returns before executing it. In type annotations, we use the `|`
operator to represent this fact.

Here's the contract for `prompt`:

```ts
function prompt(message: string): string | null;
```

The `|` operator defines a **union type**. A union represents a value
that may be one of different types: string or number, boolean or null,
etc. We refer to each of these types as the union's **members**.

Since we don't know to which member the value of a union belongs to when
writing the code, only operations that are valid for every member are
allowed. If you have the union `string | null`, you cannot use functions
that can only be applied on strings because it would cause an error when
the value is `null`.

In this case, the solution is to **narrow** the union. Narrowing occurs
when we can deduce a more specific type for a value based on the
structure of the code. The easiest way to narrow a union is with a
conditional statement. For example:

```ts
const name = prompt("What is your name?");
if (name !== null) {
    console.log("Here 'name' is an string for sure!");
}
```

_Inside_ the conditional block, we can assert that `name` is a string
because the block is only executed if `name` is not `null`.

## Early return

In a function, **early returns** (also known as **guard clauses**) can
be used to narrow a union. As its name suggests, an early return is a
conditional statement that handles _unwanted_ cases by returning
_early_. They're often used to improve readability by preventing the
_desired_ execution flow from being nested inside a conditional.

In the `greet` function below, for example, a conditional is used to
check whether `name` is `null`. If so, the `return` statement is
executed, and the function exits before the call to `console.log`.

```ts
function greet(): void {
    const name = prompt("What is your name?");
    if (name === null) return; // early return
    console.log("Here 'name' is an string for sure!");
}
```

Because the conditional is flipped (i.e., we check if `name` _is_ `null`
rather than if it is _not_ `null`), `name` is guaranteed to be a string
if the test expression is false. In that sense, early returns act as
_filters_. They handle undesirable conditions early, allowing the rest
of the function to follow the "happy path" where everything works as
expected.
