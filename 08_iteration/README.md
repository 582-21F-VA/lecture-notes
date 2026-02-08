# Iteration

To iterate means to repeat an operation a certain number of times. Most
often, it refers to going through a collection of values and performing
a series of actions on each one. To do this, JavaScript offers two types
of constructs: **loops** such as `for` and `for..of`, and **higher-order
functions** such as `map`, `filter`, and `reduce`. In most cases, these
constructs can be used interchangeably, but higher-order functions avoid
mutation and are often more concise.

## Loops

Consider a program that finds all even numbers in a list. One way to
write this is as follows:

```ts
const numbers = [1, 2, 3, 4, 5];
const evens = [];

if (numbers[0] % 2 === 0) evens.push(numbers[0]);
if (numbers[1] % 2 === 0) evens.push(numbers[1]);
if (numbers[2] % 2 === 0) evens.push(numbers[2]);
if (numbers[3] % 2 === 0) evens.push(numbers[3]);
if (numbers[4] % 2 === 0) evens.push(numbers[4]);

console.log(evens); // => [0, 2, 4]
```

> [!NOTE]
> Using the remainder (`%`) operator is an easy way to test whether a
> number is divisible by another number. If it is, the remainder of
> their division is zero.

That works, but the idea of writing a program is to make something less
work, not more. If the list contained 1,000 numbers, this approach would
be unworkable. What we need is a way to run a piece of code multiple
times. This form of control flow is called a loop.

Looping control flow allows us to go back to some point in the program
where we were before and repeat it with our current program state. If we
combine this with a binding that counts, we can do something like this:

```ts
const numbers = [1, 2, 3, 4, 5];
const evens = [];

let i = 0;

while (i < numbers.length) {
    const n = numbers[i];
    const isEven = n % 2 === 0;
    if (isEven) evens.push(n);
    i++;
}
```

A statement starting with the keyword `while` creates a loop. The word
`while` is followed by an expression in parentheses and then a
statement. The loop keeps entering that statement as long as the
expression produces a value that gives `true` when converted to boolean.

```
while (<condition>) <statement>
```

The `i` variable used above demonstrates the way a binding can track the
progress of a program. Every time the loop repeats, `i` gets a value
that is 2 more than its previous value. At the beginning of every
repetition, it is compared to the length of the numbers list to decide
whether the program's work is finished.

Many loops follow the pattern shown in the `while` example. First a
"counter" binding is created to track the progress of the loop. Then
comes a `while` loop, usually with a condition that checks whether the
counter has reached its end value. At the end of the loop body, the
counter is updated to track progress.

Because this pattern is so common, JavaScript provide a slightly shorter
and more comprehensive form, the `for` loop:

```ts
const numbers = [1, 2, 3, 4, 5];
const evens = [];

for (let i = 0; i < numbers.length; i++) {
    const n = numbers[i];
    const isEven = n % 2 === 0;
    if (isEven) evens.push(n);
}
```

This program is exactly equivalent to the earlier example. The only
change is that all the statements that are related to the state of the
loop are grouped together after `for`.

The parentheses after a `for` keyword must contain two semicolons. The
part before the first semicolon _initializes_ the loop, usually by
defining a binding. The second part is the expression that _checks_
whether the loop must continue. The final part _updates_ the state of
the loop after every iteration.

```
for (<initialization>; <condition>; <update>) <statement>
```

In most cases, this is shorter and clearer than a `while` construct.

### Breaking out of loops

Having the looping condition produce `false` is not the only way a loop
can finish. The `break` statement has the effect of immediately jumping
out of the enclosing loop. Its use is demonstrated in the following
program, which only finds the _first_ even number:

```ts
const numbers = [1, 2, 3, 4, 5];
const evens = [];

for (let i = 0;; i++) {
    const n = numbers[i];
    const isEven = n % 2 === 0;
    if (isEven) {
        evens.push(n);
        break;
    }
}
```

The `for` construct in the example does not have a part that checks for
the end of the loop. This means that the loop will never stop unless the
`break` statement inside is executed.

If you were to remove that `break` statement or you accidentally write
an end condition that always produces `true`, your program would get
stuck in an **infinite loop**. A program stuck in an infinite loop will
never finish running, which is usually a bad thing.

The `continue` keyword is similar to `break` in that it influences the
progress of a loop. When `continue` is encountered in a loop body,
control jumps out of the body and continues with the loop's next
iteration.

### For..of

If you copy the previous examples in your text editor, you will most
likely see an error about `n` being possible undefined. That is because
in JavaScript indexing an array results in an undefined value if the
given index does not correspond to an element in the array. The value of
`numbers[7]`, for instance, is `undefined` because `numbers` has only 5
elements.

We could use a guard clause to make sure that `n` is not undefined, but
a better solution is to use another kind of loop:

```ts
const numbers = [1, 2, 3, 4, 5];
const evens = [];

for (const n of numbers) {
    const isEven = n % 2 === 0;
    if (isEven) evens.push(n);
}
```

In contrast with a regular `for` loop, a `for..of` loop automatically
iterates over each element of an array. In the example above, the
constant `n` refers in turn to every number in `numbers`. Because of
this, it will never be undefined.

## Higher-order functions

Higher-order functions are functions that either take one or more
functions as arguments, or return a function. While regular functions
(also known as **first-order functions**) let us abstract over _values_,
higher-order functions let us abstract over _operations_.

The previous program, for example, repeatedly checked whether a given
number was even, and used the result to filter a list of numbers.
Checking whether a given number is even or not is an operation that can
be abstracted in a function:

```ts
/** Determine if a number n is even. */
function isEven(n: number): boolean {
    return n % 2 === 0;
}

expect(isEven(2)).toBe(true);
expect(isEven(3)).toBe(false);
```

This function can then be passed to a higher-order function such as
`filter`, which will return a new array containing only even numbers:

```ts
const numbers = [1, 2, 3, 4, 5];
const evens = numbers.filter(isEven);
```

Under the hood, `filter` works roughly like a loop. In fact, we could
define our own version of `filter` to better understand how it works:

```ts
function filter(array, predicate) {
    const results = [];
    for (const element of array) {
        if (predicate(element)) results.push(element);
    }
    return result;
}
```

As you can see, the body of `filter` is similar to our previous program,
but for the fact that the condition is abstracted into the `predicate`
function.

Another higher-order function called `map` works similarly to `filter`,
but _transforms_ an array instead of filtering it. For example, we can
use `map` to add a question mark to every string in an array:

```ts
const questions = ["a", "b", "c"].map(s => s + "!");
console.log(questions); // => ["a!", "b!", "c!"]
```

Like `filter`, `map` takes a function as an argument. This time, we use
the arrow function `s => s + "!"`. This function is applied in turn to
every element of the array, and the return values are inserted into a
new array.

Again, we can define our own version of `map` to understand how it
works:

```ts
function map(array, transformer) {
    const results = [];
    for (const element of array) {
        const result = transform(element);
        results.push(result);
    }
    return results;
}
```

Note that the `transformer` function that is passed to `map` must always
return a value. Otherwise, `undefined` would be inserted into the
resulting array.

JavaScript includes other useful higher-order functions such as
[reduce], [every], and [some]. We strongly recommend becoming familiar
with them, as they can simplify operations that would otherwise require
verbose loops.

[reduce]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce
[every]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every
[some]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some
