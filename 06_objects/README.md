# Objects

Numbers, booleans, and strings are the primitive values from which data
structures are built. Many types of information require more than one
value, though. **Objects** allow us to group values — including other
objects — to build more complex structures. In JavaScript, everything
that is not a primitive value is an object. Even numbers, booleans and
strings, which are technically not objects, sometimes act as if they
were.

An object is a composite data type that contains a set of
**properties**. These properties define the characteristics and the
behavior of the object they belong to. An object representing a bank
account, for instance, might have properties for its holder and balance.
Properties are variable bindings; they have a name and a value. A
property whose value is a function is referred to as a **method**.

The simplest way to create an object is to write down its properties in
between curly brackets. This is called the **object literal** syntax,
because we _literally_ describe the object's structure.

```ts
const kenAccount = {
    holder: "Ken Takakura",
    balance: 0,
    deposit: function(amount: number): number {
        return kenAccount.balance += amount;
    },
};
```

Here, we've created an object with three properties (`holder`,
`balance`, `deposit`), and assigned it the name `kenAccount`. The value
of `deposit` is a function, meaning that it's a method.

You can access the value of a property using **dot notation**.

```ts
console.log(kenAccount.holder); // => "Ken Takakura"
```

## Mutability and references

Primitive values such as numbers, strings and booleans are
**immutable**, that is, they cannot be modified. You can combine them
and derive new values from them, but when you take a specific string
value, that value will always remain the same. The text inside it cannot
be changed. If you have a string that contains `"cat"`, it is not
possible for other code to change a character in your string to make it
spell `"rat"`.

Objects work differently. You can change their properties, causing a
single object value to have different content at different times.

When we have two numbers, `120` and `120`, we can consider them
precisely the same number, whether or not they refer to the same
physical bits. With objects, there is a difference between having two
_references_ to the same object and having two different objects that
contain the same properties.

Consider the following code:

```ts
const a = { n: 1 };
const b = a;
const c = { n: 1 };

console.log(a === b); // => true
console.log(a === c); // => false

a.n = 2;

console.log(b.n); // => 2
```

The `a` and `b` bindings grasp the same object, which is why changing
`a` also changes the value of `b`. They are said to have the same
**identity**. The binding `c` points to a different object, which
initially contains the same properties as `a` but lives a separate life.

When you compare objects with JavaScript's `===` operator, it compares
by identity: it will produce `true` only if both objects are precisely
the same value. Comparing different objects will return `false`, even if
they have identical properties. There is no "deep" comparison operation
built into JavaScript that compares objects by contents.

## Object types

In JavaScript, `object` is a data type, just like `string` and `number`.
The value of the expression `typeof { n: 1 }`, for instance, is
`"object"`. But we also distinguish between different **object types**
based on their properties. Hence objects of type `{ n: number }`, which
have a property `n` whose value is a number, are different from objects
of type `{ s: string }`, which have a property `s` whose value is a
string.

Although you can use these types directly to document functions, they're
often given a name. Objects of the built-in `Date` type, for instance,
have a method called `getFullYear`, which returns the year of the date
the object represents.

To create a `Date` object, we invoke the `Date` function, preceded by
the `new` keyword. A function such as `Date` that creates objects of a
specific type is called a **constructor**.

```ts
const nationalHoliday = new Date(2025, 5, 24);
console.log(nationalHoliday.getFullYear()); // => 2025
```

Since `Date` is the name of a type, we can use it to annotate function
parameters or return values. For example, the function `isBeforeToday`
implemented below checks whether a given date comes before today's date.

```ts
function isBeforeToday(d: Date): boolean {
    return d < Date.now();
}
```

Eventually, we'll cover how to create our own object types, but for now
we'll stick to using the standard built-in types that come with
JavaScript.
