# Bindings

A fragment of code that produces a value is called an **expression**.
Every value that is written literally (such as `22` or
`"psychoanalysis"`) is an expression. An expression between parentheses
is also an expression, as is a binary operator applied to two
expressions, or a unary operator applied to one.

This shows part of the beauty of a language-based interface. Expressions
can contain other expressions in a way similar to how sub-sentences in
human languages are nested — a sub-sentence can contain its own
sub-sentences, and so on. This allows us to build expressions that
describe arbitrarily complex computations.

If an expression corresponds to a sentence fragment, a JavaScript
**statement** corresponds to a full sentence. A **program** is a list of
statements.

The simplest kind of statement is an expression with a semicolon after
it. This is a program:

```ts
1;
!false;
```

It is a useless program, though. An expression can be content to just
produce a value, which can then be used by the enclosing code. However,
a statement stands on its own, so if it doesn't affect the world, it's
useless. It may display something on the screen, as with `console.log`,
or change the state of the machine in a way that will affect the
statements that come after it. These changes are called **side
effects**. The statements in the previous example just produce the
values `1` and `true` and then immediately throw them away. This leaves
no impression on the world at all. When you run this program, nothing
observable happens.

So how does a program keep an internal state? How does it remember
things? We have seen how to produce new values from old values, but this
does not change the old values, and the new value must be used
immediately or it will dissipate again. To catch and hold values,
JavaScript provides a thing called a **binding**.

```ts
const caught = 5 * 5;
```

This example creates a binding called `caught`, and uses it to grab hold
of the number that is produced by multiplying 5 by 5.

After a binding has been defined, its name can be used as an expression.
The value of such an expression is the value the binding currently
points to. Here's an example:

```ts
const ten = 10;
console.log(ten * ten); // => 20
```

> [!NOTE]
> In the notes, to show the output that a statement produces, a comment
> is written after it with an arrow and the expected output. In
> JavaScript, two slash characters are used to introduce single-line
> comments. A comment is a piece of text that is part of a program but
> is completely ignored by the computer.

You should imagine bindings as labels rather than boxes. Bindings do not
_contain_ values; they _point_ to them. More than one binding can refer
to the same value, but a program can access only values to which it
still has a reference. For that reason, when you need to remember
something, you put on label on it.

There are two types of bindings in JavaScript: **constants** and
**variables**. The keyword `const` indicates that a statement is going
to define a _constant_ binding. Once a constant has been assigned to a
value, it is tied to that value until the end of the program. The
keyword `let` indicates that a statement is going to define a _variable_
binding. In contrast with constants, variables can be disconnected from
their current value and pointed to a new one.

```ts
let mood = "light";
console.log(mood); // => "light"

mood = "dark";
console.log(mood); // => "dark"
```

Let's look at another example. To remember the number of dollars that
Luigi still owes you, you create a binding. When he pays back $35, you
give this binding a new value.

```ts
let luigisDebt = 140;
luigisDebt = luigisDebt - 35;
console.log(luigisDept); // => "105"
```

> [!NOTE]
> Binding names may not contain spaces, yet it is often helpful to use
> multiple words to clearly describe what the binding represents. To do
> so, most JavaScript programs use _camel casing_, where every word is
> capitalized except the first.

The collection of bindings and their values that exist at a given time
is called the _environment_. When a program starts up, this environment
is not empty. It always contains bindings that are part of the language
standard, and most of the time, it also has bindings that provide ways
to interact with the surrounding system. For example, in a browser,
there are functions to interact with the currently loaded website and to
read mouse and keyboard input.
