# Propagation

For most event types, handlers registered on nodes with children will
also receive events that happen in the children. If a button inside a
paragraph is clicked, event handlers on the paragraph will also see the
click event.

```html
<p>A paragraph with a <button>button</button>.</p>
```

```ts
const p = document.querySelector("p");
p?.addEventListener("click", () => {
    console.log(
        "This handler will be called when I click on the button.",
    );
});
```

If both the paragraph and the button have a handler, the more specific
handler (in this case, the one on the button) gets to go first. The
event is said to **propagate** outward from the node where it happened
to that node's parent node and on to the root of the document. Finally,
after all handlers registered on a specific node have had their turn,
handlers registered on the whole window get a chance to respond to the
event.

```ts
const button = querySelector("button");
button?.addEventListener("click", () => {
    console.log("I get to go first!");
});

window.addEventListener("click", () => {
    console.log("I get to go last :(");
});
```

At any point, an event handler can call the `stopPropagation` method on
the event object to prevent handlers further up from receiving the
event. This can be useful when, for example, you have a button inside
another clickable element and you don't want clicks on the button to
activate the outer element's click behavior.

```ts
button?.addEventListener("click", (event) => {
    console.log("I get to go first!");
    event.stopPropagation(); // stop the event from propagating outward
});
```

## Target

Most event objects have a `target` property that refers to the node
where they originated. You can use this property to ensure that you're
not accidentally handling something that propagated up from a node you
do not want to handle.

It is also possible to use the `target` property to cast a wide net for
a specific type of event. For example, if you have a node containing a
long list of buttons, it may be more convenient to register a single
click handler on the outer node and have it use the `target` property to
figure out whether a button was clicked, rather than registering
individual handlers on all of the buttons.

```html
<div>
    <button>A</button>
    <button>B</button>
    <button>C</button>
</div>
```

```ts
const div = document.querySelector("div");
div.addEventListener("click", (event) => {
    if (!(event.target instanceof HTMLButtonElement)) return;
    console.log(`Clicked on button ${event.target.textContent}`);
});
```

Above, we use the `instanceof` operator to filter out click events whose
target is not a button element. As its name suggests, `instanceof`
expressions are true only if the object on the left-hand side of the
operator is an instance of the type on the right-hand side. Here, we are
only interested in click events whose target is an instance of
`HTMLButtonElement`. The callback function will return early if we
clicked on the `<div>` itself, which is an instance not of
`HTMLButtonElement`, but `HTMLDivElement`.
