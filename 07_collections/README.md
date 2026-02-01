# Collections

JavaScript does not have an explicit data type for representing lists.
However, the language do offer various object types for collecting
values. These are commonly referred to as **collections**.

## Array

Objects of type `Array` are used to store sequences of values that can
be accessed by their index. To create a new array, we can use the
`Array` constructor or the array literal syntax.

```ts
const numbers = new Array(1, 2, 3);
console.log(numbers); // => [1, 2, 3]
const letters = ["a", "b", "c"];
console.log(letters); // => ["a", "b", "c"]
```

To refer to an element in an array, we use the accessor `[<index>]`,
where `<index>` is an expression whose value corresponds to the position
of the desired element. Elements are indexed starting from 0: the first
element has index 0, the second has index 1, and so on.

```ts
console.log(numbers[0]); // => 1
console.log(letters[1]); // => "b"
```

The index provided to `[]` must be a non-negative integer. To access an
element from the end of the array, the `at` method is used instead.

```ts
console.log(numbers.at(-1)); // => 3
console.log(letters.at(-2)); // => "b"
```

Since arrays are objects, they are mutable. The `[]` accessor can be
used not only to access an element, but also to change its value.

```ts
letters[0] = "à";
console.log(letters); // => ["à", "b", "c"]
```

Similarly, `push`, `pop`, `unshift` and `shift` are methods that modify
the array they are called on. Below, notice that `numbers` is never
reassigned; it is the same array that changes over time.

```ts
// push: adds an element to the end of the array
numbers.push(4);
console.log(numbers); // => [1, 2, 3, 4]

// pop: removes the last element of the array
numbers.pop();
console.log(numbers); // => [1, 2, 3]

// unshift: adds an element to the beginning of the array
numbers.unshift(0);
console.log(numbers); // => [0, 1, 2, 3]

// shift: removes the first element of the array
numbers.shift();
console.log(numbers); // => [1, 2, 3]
```

Mutating an object within the same environment where it was created is
acceptable, but it's best to avoid it if the object comes from outside
the local environment. For example, the function `f` below modifies the
array `a` it is invoked with, even though that array was created outside
its body.

```ts
function f(a: Array<number>): void {
    a.push(4);
}

f(numbers);
console.log(numbers); // => [1, 2, 3, 4]
```

> [!NOTE]
> The type annotation `Array<number>` indicates that the parameter `a`
> is an `Array` object containing numbers. An equivalent notation is
> `number[]`.

This kind of code can be difficult to debug, as it's hard to track how
`numbers` changes over time.

In such cases, the `slice` and `concat` methods are good alternatives
since they return a new array rather than modifying the original.

```ts
// slice: returns a copy of a portion of the array
console.log([1, 2, 3].slice(0, -1)); // => [1, 2]
console.log([1, 2, 3].slice(1)); // => [2, 3]

// concat: concatenates two arrays
console.log([1, 2].concat([3])); // => [1, 2, 3]
console.log([1].concat([2, 3])); // => [1, 2, 3]
```

## Set

Objects of type `Set` allow us to store sequences of _unique_ values. To
create a new set, we use the `Set` constructor, to which we pass an
array containing the elements of the set. In the example below, notice
how `lettersSet` contains the value `"a"` only once, even though it
appears twice in the array.

```ts
const lettersSet = new Set(["a", "a", "b", "c"]);
console.log(lettersSet); // => Set(3) { "a", "b", "c" }
```

To modify a set, we use the `add` and `delete` methods:

```ts
lettersSet.add("d");
console.log(lettersSet); // => Set(4) { "a", "b", "c", "d" }

lettersSet.delete("b");
console.log(lettersSet); // => Set(3) { "a", "c", "d" }
```

## Map

Object of type `Map` (known in other languages as **hash maps** or
**dictionaries**) are used to store key-value pairs. They are especially
useful when you need to add or remove pairs frequently. Additionally,
any type of value can be used as a key.

To create a map, we use the `Map` constructor, to which we pass an array
whose elements are key-value pair arrays.

```ts
const numbersMap = new Map([
    [1, "un"],
    [2, "deux"],
]);
```

We use the methods `get`, `set`, and `delete` to retrieve, add and
remove pairs:

```ts
console.log(numbersMap.get(1)); // => "un"
numbersMap.set(3, "trois");
console.log(numbersMap); // => Map(3) { 1 => "un", 2 => "deux", 3 => "trois" }
numbersMap.delete(1);
console.log(numbersMap); // => Map(2) { 2 => "deux", 3 => "trois" }
```

Note that all methods for modifying sets and maps change them _in
place_. Unlike arrays, there are no methods like `slice` or `concat` for
these object types. If you don't want to mutate the original, you need
to create a copy first. To do so, we pass the original object to its
constructor.

```ts
function copyLettersSet(s: Set<string>): Set<string> {
    return new Set(s);
}

function copyNumbersMap(m: Map<number, string>): Map<number, string> {
    return new Map(m);
}
```
