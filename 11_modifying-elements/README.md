# Modifying elements

In HTML, the behavior of an element is configured through its
**attributes**. An `<input>` element, for instance, takes different
shapes depending on the value of its `type` attribute. Similarly, the
`required` attribute dictates whether a field is mandatory or not for
submitting the form it belongs to.

```html
<input type="email" required>
```

In JavaScript, the behavior of a node is configured through its
**properties**, the name of which often mirrors the element's
attributes.

```ts
const input = document.createElement("input");
input.type = "email";
input.required = true;
console.log(input.outerHTML); // '<input type="email" required="">'
```

The two snippets above (the first in HTML, the second in JavaScript)
produce the same element.

> [!NOTE]\
> The examples use the `createElement` method, which creates a new
> element, and the `outerHTML` property, whose value represents the
> element's markup. These properties will be explained in more detail
> later.

## Style

In HTML, the `style` attribute can be used to modify an element's
appearance.

```html
<p style="color: red">I am red.</p>
```

In JavaScript, the `style` property of a node is a `CSSStyleDeclaration`
object that contains style properties. These style properties generally
have the same name as their CSS counterparts, except for properties that
contain a hyphen. For example, the CSS property `color` is named `color`
in JavaScript, but `background-color` is translated as
`backgroundColor`.

```ts
const p = document.createElement("p");
p.style.color = "red";
p.style.backgroundColor = "blue";
console.log(p.outerHTML); // '<p style="color: red; background-color: blue;"></p>'
```

## Classes

The `classList` property represents the CSS classes of the receiver
node. Its value is an object of type `DOMTokenList` that provides
various methods for manipulating classes.

The `add` and `remove` methods, for instance, are used to add and remove
one or more classes.

```ts
const h1 = document.createElement("h1");
h1.classList.add("foo", "bar");
console.log(h1.outerHTML); // => '<h1 class="foo bar"></h1>'

h1.classList.remove("foo");
console.log(h1.outerHTML); // => '<h1 class="bar"></h1>'
```

Similarly, the `toggle` and `replace` methods are used to toggle a given
class and to replace one class with another.

```ts
const h2 = document.createElement("h2");
console.log(h2.outerHTML); // '<h2></h2>'
h2.classList.toggle("foo");
console.log(h2.outerHTML); // '<h2 class="foo"></h2>'
h2.classList.toggle("foo");
console.log(h2.outerHTML); // '<h2 class=""></h2>'

h2.classList.add("foo");
console.log(h2.outerHTML); // '<h2 class="foo"></h2>'
h2.classList.replace("foo", "bar");
console.log(h2.outerHTML); // '<h2 class="bar"></h2>'
```

## Text content

The `textContent` property of a node represents its text content and the
text content of its descendants.

```ts
const span = document.createElement("span");
span.textContent = "exemple";
console.log(span.outerHTML); // '<span>exemple</span>'
console.log(span.textContent); // exemple
```
