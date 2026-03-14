# Forms

Web forms are one of the main points of interaction between a user and a
website or application. Forms allow users to enter data, which is sent
to a web server for processing and storage, or used in the browser to
immediately update the interface in some way (e.g. to add another item
to a list, or show or hide a UI feature).

A web form's HTML is made up of one or more **form controls** (sometimes
called **widgets**), plus some additional elements to help structure the
overall form. The controls can be single or multi-line text fields
(`<input>`, `<textarea>`), dropdown boxes (`<select>`), checkboxes
(`<input type="checkbox">`), or radio buttons (`<input type="radio">`).
They are _always_ paired with a text label (`<label>`) that describe
their purpose to both sighted and visually impaired users.

> [!NOTE]\
> The `placeholder` attribute defines the text displayed in a form
> control when the control has no value. It can be used to provide a
> brief hint as to the expected type of data that should be entered into
> the control, but it must never be used instead of a `<label>`. Since
> the placeholder dissapears as soon as the user start typing, using
> `placeholder` instead of a `<label>` harms usability and
> accessibility.

## Sending form data

The `<form>` element defines how the data will be sent. All of its
attributes are designed to let you configure the request to be sent when
a user hits a submit `<button>` or presses the RETURN key on their
keyboard. The two most important attributes are `action` and `method`.

### Action

The `action` attribute defines where the data gets sent. Its value must
be a valid relative or absolute URL. If this attribute isn't provided,
the data will be sent to the URL of the page containing the form (i.e.,
the current page).

The names and values of form controls are sent to the server as
`name=value` pairs joined with ampersands (`&`). The `action` value
should point to a route on the server that can handle the incoming data.

### Method

The `method` attribute defines how data is sent. HTML form data can be
transmitted via a number of different methods, the most common being
`GET` method and the `POST` method.

#### GET

The `GET` method is the method used by the browser to ask the server to
send back (i.e. to _get_) a given resource. In this case, the browser
sends an empty body. Because the body is empty, if a form is sent using
this method, the data sent to the server is appended to the URL.

Consider the following form:

```html
<form action="/greet" method="GET">
    <label>
        What greeting do you want to say?
        <input type="text" name="say" value="Hi" />
    </label>
    <label>
        Who do you want to say it to?
        <input type="text" name="to" value="Mom" />
    </label>
    <button>Send my greetings</button>
</form>
```

Since the `GET` method has been used, you'll see the URL
`/greet?say=Hi&to=Mom` appear in the browser address bar when you submit
the form. In this case, we are passing two pieces of data to the server:

- `say`, which has a value of `Hi`; and
- `to`, which has a value of `Mom`.

The request line of the HTTP request will look like this:

```
GET /?say=Hi&to=Mom HTTP/2.0
Host: example.com
```

#### POST

The `POST` method is a little different. If a form is sent using this
method, the data is appended to the body of the HTTP request.

```html
<form action="/greet" method="POST">
    <label>
        What greeting do you want to say?
        <input name="say" type="text" value="Hi" />
    </label>
    <label>
        Who do you want to say it to?
        <input name="to" type="text" value="Mom" />
    </label>
    <button>Send my greetings</button>
</form>
```

When the form is submitted using the `POST` method, you get no data
appended to the URL. The HTTP request looks like so, with the data
included in the request body instead:

```
POST / HTTP/2.0
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13

say=Hi&to=Mom
```

The `Content-Length` header indicates the size of the body, and the
`Content-Type` header indicates the type of resource sent to the server.

## Form events

When a user attempts to submit data, the `submit` event is triggered by
the form. Since data can be submitted by pressing the RETURN key, it's
preferable to listen to this event rather than the `click` on the submit
`<button>`.

```ts
const form = document.querySelector("form");
form?.addEventListener("submit", (event) => {
    event.preventDefault(); // prevent the form submission
    console.log("The user submitted the form.");
});
```

By default, when a form is submitted, the browser sends the appropriate
request to the server, then displays the body of the response, causing a
new page load. If you don't want this to happen because you are using
the form client-side to update the interface in some way, you can use
the `preventDefault` method on the event object.

Form controls trigger events without requiring the form to be submitted:

- `focus` occurs when the control receives focus.
- `input` fires with every individual change to the control's value.
- `change` triggers when the value change is confirmed.
- `blur` occurs when the control loses focus.

You can find a [demo here][demo events] that clearly illustrates the
differences between each event (run the code snippet at the bottom of
the response).

[demo events]: https://stackoverflow.com/a/69167655

## Form data

Retrieving the values of multiple form controls can be tedious if you
select each one individually with `querySelector`. The `elements`
property of an `HTMLFormElement` provides a list of all controls within
the `<form>`, but you still need to extract their values manually.

As an alternative, the `FormData` interface provides a way to construct
a collection of key-value pairs representing form controls and their
values. To create a `FormData` object, we pass the `HTMLFormElement` to
the `FormData` constructor:

```html
<form>
    <label>
        Username
        <input name="username" type="text" value="foobar" />
    </label>
    <button>Submit</button>
</form>
```

```ts
function main(): void {
    const form = document.querySelector("form");
    if (!form) return;
    const data = new FormData(form);
}
```

We can then use the `get` method to retrieve the value of the control
with the given name.

```ts
const username = data.get("username")?.toString() ?? "";
```

The `get` method returns a union of `FormDataEntryValue | null`. You can
use `toString` to convert a `FormDataEntryValue` to a string, and use
the `??` operator to provide a default value if the form does not
contain the desired control.
