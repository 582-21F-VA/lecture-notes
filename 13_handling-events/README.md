# Handling events

Some programs work with direct user input, such as mouse and keyboard
actions. That kind of input isn't available ahead of time; it comes in
piece by piece, in real time, and the program must respond to it as it
happens.

## Event handlers

Imagine an interface where the only way to find out whether a key on the
keyboard is being pressed is to read the current state of that key. To
be able to react to keypresses, you would have to constantly read the
key's state to catch it before it is released again. It would be
dangerous to perform other time-intensive computations, since you might
miss a keypress.

A better mechanism is for the system to actively notify the code when an
event occurs. Browsers do this by allowing us to register functions as
**handlers** for specific events.

```ts
window.addEventListener("click", () => {
    console.log("You clicked in the window");
});
```

The `window` binding refers to a built-in object provided by the
browser. It represents the browser window that contains the HTML
document. Calling its `addEventListener` method registers the second
argument to be called whenever the event described by its first argument
occurs.

## Events and DOM nodes

Each browser event handler is registered in a context. In the previous
example, we called `addEventListener` on the `window` object to register
a handler for the whole window. Such a method can also be found on DOM
elements and some other types of objects. Event listeners are called
only when the event happens in the context of the object on which they
are registered.

```html
<button>Click me</button>
<p>No handler here.</p>
```

```ts
const button = document.querySelector("button");
button?.addEventListener("click", () => {
    console.log("Button clicked.");
});
```

That example attaches a handler to the `button` node. Clicks on the
button cause that handler to run, but clicks on the rest of the document
do not.

## Event objects

Though we have ignored it so far, event handler functions are passed an
argument: the event object. This object holds additional information
about the event. For example, if we want to know which mouse button was
pressed, we can look at the event object's `button` property.

```ts
const button = document.querySelector("button");
button?.addEventListener("mousedown", (event) => {
    if (event.button == 0) {
        console.log("Left button");
    } else if (event.button == 1) {
        console.log("Middle button");
    } else if (event.button == 2) {
        console.log("Right button");
    }
});
```

The information stored in an event object differs per [type of event].
We'll discuss different types later.

[type of event]: https://developer.mozilla.org/en-US/docs/Web/API/Event#interfaces_based_on_event
