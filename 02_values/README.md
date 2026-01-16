# Values

The purpose of a program is to describe a computational process that
consumes some **information** and produces new information. All this
information comes from a part of the real world — often called the
program's **domain** — and the results of a program's computation
represent more information in this domain.

Think of information as facts about the program's domain. For a program
that deals with a furniture catalog, a "table with five legs" or a
"square table of two by two meters" are pieces of information. A game
program deals with a different kind of domain, where "five" might refer
to the number of pixels per clock tick that some object travels on its
way from one part of the canvas to another. Or, a payroll program is
likely to deal with "five deductions."

For a program to process information, it must:

1. turn information into some form of **data** in the programming
   language;
2. processes the data; and
3. turn the resulting data into information again.

An interactive program may even intermingle these steps, acquiring more
information from the world as needed and delivering information in
between.

In the computer's world, there is only data. You can read data, modify
data, create new data — but that which isn't data cannot be mentioned.

All this data is stored as long sequences of **bits**, and is thus
fundamentally alike. Bits are any kind of two-valued things, usually
described as zeros and ones. A typical modern computer has more than 100
billion bits in its volatile data storage (working memory). Nonvolatile
storage (the hard disk or equivalent) tends to have yet a few orders of
magnitude more.

To be able to work with such quantities of bits without getting lost, we
separate them into chunks that represent pieces of information. In
programming, those chunks are called **values**. Though all values are
made of bits, they play different roles. Every value has a **type** that
determines its role. Some values are numbers, some values are pieces of
text, some values are functions, and so on.

The simplest of these values are called **primitives**. The following
sections introduces JavaScript's primitives as well as the operators
that can act on them.

## Numbers

Values of the **number** type are, unsurprisingly, numeric values. In a
JavaScript program, they are written as follows:

```
13
```

The main thing to do with numbers is arithmetic. Arithmetic operations
such as addition or multiplication take two number values and produce a
new number from them. Here is what they look like in JavaScript:

```
100 + 4 * 11
```

The `+` and `*` symbols are called **operators**. The first stands for
addition, and the second stands for multiplication. Putting an operator
between two values will apply it to those values, and produce a new
value.

As in mathematics, you can change the order in which operations are
applied by wrapping them in parentheses.

```
(100 + 4) * 11
```

For subtraction, there is the `-` operator. Division can be done with
the `/` operator. There is one more arithmetic operator, which you might
not immediately recognize. The `%` symbol is used to represent the
remainder operation. You'll also often see this operator referred to as
"modulo". `X % Y` is the remainder of dividing `X` by `Y`. For example,
`314 % 100` produces `14`, and `144 % 12` gives `0`.

## Strings

The next basic data type is the **string**. Strings are used to
represent text. They are written by enclosing their content in quotes.

```
`Down on the sea`
"Lie on the ocean"
'Float on the ocean'
```

You can use single quotes, double quotes, or backticks to mark strings,
as long as the quotes at the start and the end of the string match.

Strings cannot be divided, multiplied, or subtracted. The `+` operator
can be used on them, not to add, but to **concatenate** — to glue two
strings together. The following line will produce the string
`"concatenate"`:

```
"con" + "cat" + "e" + "nate"
```

Strings written with single or double quotes behave very much the same,
but backtick-quoted strings, usually called **template literals**, can
do a few more tricks. Apart from being able to span lines, they can also
embed other values.

```
`half of 100 is ${100 / 2}`
```

When you write something inside `${}` in a template literal, its result
will be computed, converted to a string, and included at that position.
This example produces the string `"half of 100 is 50"`.

## Unary operators

Not all operators are symbols. Some are written as words. One example is
the `typeof` operator, which produces a string value naming the type of
the value you give it.

```
typeof 123
```

The other operators shown so far operated on two values, but `typeof`
takes only one. Operators that use two values are called **binary
operators__, while those that take one are called **unary operators**.
The minus operator (`-`) can be used both as a binary operator and as a
unary operator.

```
0 - 10
-10
```

## Booleans

It is often useful to have a value that distinguishes between only two
possibilities, like "yes" and "no" or "on" and "off". For this purpose,
JavaScript has a **boolean** type, which has just two values, `true` and
`false`, written as those words.

### Comparison operators

Here is one way to produce boolean values:

```
3 > 2
```

The `>` and `<` signs are the traditional symbols for "is greater than"
and "is less than", respectively. They are binary operators. Applying
them results in a boolean value that indicates whether they hold true in
this case.

Strings can be compared similarly.

```
"Aardvark" < "Zoroaster"
```

The way strings are ordered is roughly alphabetic but not really what
you'd expect to see in a dictionary: uppercase letters are always "less"
than lowercase ones, so `"Z" < "a"`, and non-alphabetic characters (`!`,
`-`, and so on) are also included in the ordering. When comparing
strings, JavaScript goes over the characters from left to right,
comparing characters one by one.

Other similar operators are `>=` (greater than or equal to), `<=` (less
than or equal to), `===` (equal to), and `!==` (not equal to).

```
"Garnet" !== "Ruby"
"Pearl" === "Amethyst"
```

> [!NOTE]\
> Avoid using the `==` and `!=` operators. They allow you to compare
> values of different types, but can produce bugs that are difficult to
> find.

### Logical operators

There are also some operations that can be applied to boolean values
themselves. JavaScript supports three logical operators: **and**,
**or**, and **not**. These can be used to "reason" about booleans.

The `&&` operator represents logical **and**. It is a binary operator,
and its result is `true` only if _both_ the values given to it are
`true`.

```
true && false
true && true
```

The `||` operator denotes logical **or**. It produces `true` if _either_
of the values given to it is `true`.

```
false || true
false || false
```

Logical **not** is written as an exclamation mark (`!`). It is a unary
operator that flips the value given to it — `!true` produces `false` and
`!false` gives `true`.

The last logical operator we will look at is not unary, not binary, but
**ternary**, operating on _three_ values. It is written with a question
mark and a colon, like this:

```
true ? 1 : 2
false ? 1 : 2
```

This one is called the **conditional operator** (or sometimes just the
**ternary operator** since it is the only such operator in the
language). This operator uses the value to the left of the question mark
to decide which of the two other values to "pick". If you write
`a ? b : c`, the result will be `b` when `a` is `true`, and `c`
otherwise.

### Short-circuiting

The logical operators `&&` and `||` handle values of different types in
a peculiar way. They will convert the value on their left side to
boolean type in order to decide what to do, but depending on the
operator and the result of that conversion, they will produce the
_original_ left-hand or right-hand value.

The `||` operator, for example, will return the value to its left when
that value can be converted to `true`, and will return the value on its
right otherwise. This has the expected effect when the values are
boolean and does something analogous for values of other types.

```
null || "user"
"Agnes" || "user"
```

We can use this functionality as a way to fall back on a default value.
If you have a value that might be empty, you can put `||` after it with
a replacement value. If the initial value can be converted to `false`,
you'll get the replacement instead. The rules for converting strings and
numbers to boolean values state that `0`, `NaN` (Not a Number), and the
empty string (`""`) count as `false`, while all the other values count
as `true`. That means `0 || -1` produces `-1`, and `"" || "!?"` yields
`"!?"`.

The `??` operator resembles `||` but returns the value on the right only
if the one on the left is `null` or `undefined`, not if it is some other
value that can be converted to `false`. Often, this is preferable to the
behavior of `||`.

```
0 || 100
0 ?? 100
null ?? 100
```

The `&&` operator works similarly but the other way around. When the
value to its left is something that converts to `false`, it returns that
value, and otherwise it returns the value on its right.

Another important property of these two operators is that the part to
their right is evaluated only when necessary. In the case of
`true || X`, no matter what `X` is — even if it's a piece of program
that does something terrible — the result will be `true`, and `X` is
never evaluated. The same goes for `false && X`, which is `false` and
will ignore `X`. This is called **short-circuit evaluation**.

The conditional operator works in a similar way. Of the second and third
values, only the one that is selected is evaluated.

## Empty values

There are two special values, written `null` and `undefined`, that are
used to denote the absence of a meaningful value. They are themselves
values, but they carry no information.

Many operations in the language that don't produce a meaningful value
yield `undefined` simply because they have to yield some value.

The difference in meaning between `undefined` and `null` is an accident
of JavaScript's design, and it doesn't matter most of the time. In cases
where you actually have to concern yourself with these values, you can
treat them as mostly interchangeable.
