# Creating elements

In addition to modifying existing elements, JavaScript allows you to
create new elements and insert them into the DOM. To do this, we use the
`createElement` method of the global `document` object:

```ts
const p = document.createElement("p");
console.log(p); // => HTMLParagraphElement { ... }
```

The `createElement` method creates an HTML element whose type
corresponds to the tag name specified as an argument. It returns an
object that represents the newly created element.

## Adding elements to the DOM

An element created with `createElement` is not automatically inserted
into the document. We need to connect the element to a node that is
already in the DOM. Several methods allow you to do this. The `prepend`
and `append` methods, for instance, add a set of nodes before the first
child or after the last child of the receiver element:

```ts
const span = document.createElement("span");
p.append(span);
console.log(p.outerHTML); // => "<p><span></span></p>"

const i = document.createElement("i");
p.prepend(i);
console.log(p.outerHTML); // => "<p><i></i><span></span></p>"
```

Similarly, the `before` and `after` methods add a set of nodes before of
after the receiver element:

```ts
const b = document.createElement("b");
i.after(b);
console.log(p.outerHTML); // => "<p><i></i><b></b><span></span></p>"

const em = document.createElement("em");
b.before(em);
console.log(p.outerHTML); // => "<p><i></i><em></em><b></b><span></span></p>"
```

You can also use the `replaceWith` method to replace an element in the
DOM with another, or the `replaceChildren` method to replace all
children of the receiver node with a new set of children:

```ts
const strong = document.createElement("strong");
span.replaceWith(strong);
console.log(p.outerHTML); // "<p><i></i><em></em><b></b><strong></strong></p>"

const a = document.createElement("a");
p.replaceChildren(a);
console.log(p.outerHTML); // "<p><a></a></p>"
```

Finally, the `remove` method simply removes an element:

```ts
a.remove();
console.log(p.outerHTML); // "<p></p>"
```

> [!NOTE]
> Avoid using the `innerHTML` property to modify the DOM. Its use is
> inefficient because the browser needs to parse the given HTML. Prefer
> manipulating the DOM directly with the methods listed above.
