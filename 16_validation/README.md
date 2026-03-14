# Form validation

Good forms provide feedback when users don't enter their data in the
correct format — this is called **form validation**.

But we want to make filling out web forms as easy as possible. So why do
we insist on validating our forms? There are three main reasons:

- We want to get the right data, in the right format. Our applications
  won't work properly if our users' data is stored in the wrong format,
  is incorrect, or is omitted altogether.
- We want to protect our users' data. Forcing our users to enter secure
  passwords makes it easier to protect their account information.
- We want to protect ourselves. There are many ways that malicious users
  can misuse unprotected forms to damage the application.

Validation done in the browser is called **client-side validation**,
while validation done on the server is called **server-side
validation**. Client-side validation is an initial check and an
important feature of good user experience; by catching invalid data on
the client-side, the user can fix it straight away. If it gets to the
server and is then rejected, a noticeable delay is caused by a round
trip to the server and then back to the client-side to tell the user to
fix their data. But client-side validation should not be considered an
exhaustive measure. Your apps should always perform validation the
server-side as well, because client-side validation is too easy to
bypass.

## Client-side validation

There are two different types of client-side validation:

- HTML form validation: HTML form attributes such as `required` and
  `type` can define which form controls are required and which format
  the user-entered data must be in to be valid.
- JavaScript form validation: JavaScript is generally included to
  enhance or customize HTML form validation.

Client-side validation can be accomplished with little to no JavaScript.
HTML validation is faster than JavaScript, but is less customizable. It
is recommended to begin validating your forms using the built-in HTML
validation features, then enhance the user experience with JavaScript as
needed.

### HTML validation

HTML validation is is done by using validation attributes on control
elements. Here are the attributes we can use:

- The `required` attribute specifies whether a form field needs to be
  filled in before the form can be submitted.
- The `minlength` and `maxlength` attributes specify the minimum and
  maximum length of textual data.
- The `min`, `max`, and `step` attributes specifiy the minimum and
  maximum values of numerical inputs, and the increment, or step, for
  values, starting from the minimum.
- The `type` attribute specifies whether the data needs to be a number,
  an email address, or some other specific preset type.
- The `pattern` attribute specifies a [regular expression] that defines
  a pattern the entered data needs to follow.

If the data follows all of the rules specified by the attributes applied
to the field, it is considered valid. If not, it is considered invalid.

[regular expression]: https://en.wikipedia.org/wiki/Regular_expression

### JavaScript validation

In JavaScript, the objects that represent form elements such as
`HTMLFormControl`, `HTMLInputElement` and `HTMLSelectElement` have
[various properties][Validation API] for managing validation and
customizing user feedback. Below are examples of commonly used ones.

[Validation API]: https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Form_validation#the_constraint_validation_api

#### checkValidity

The `checkValidity` method is available on both form and control
elements. When called on a control, it returns returns `true` if the
control is valid, and `false` otherwise.

```html
<form>
    <label>
        Password
        <input
            type="password"
            name="password"
            value="abc"
            minlength="8"
            required
        />
    </label>
</form>
```

```ts
const input = document.querySelector("input");
console.log(input?.checkValidity()); // => false
```

The `<input>` in the example above is currently invalid because its
value doesn't contain the minimum of 8 characters set by the `minlength`
property.

If `checkValidity` is called on a form element, it returns `true` only
if the value of _all_ controls within that form is valid.

```ts
const form = document.querySelector("form");
console.log(form?.checkValidity()); // => false
```

If a control element is invalid when `checkValidity` is called, the
control fires an `invalid` event.

```ts
input?.addEventListener("invalid", () => {
    alert("Invalid input");
});
```

#### setCustomValidity

The `setCustomValidity` method is used to add a custom error message to
the receiver control element. When you set a custom error message, the
element is considered to be invalid. This lets you use JavaScript code
to establish a validation failure other than those offered by the
standard HTML validation constraints.

```ts
const input = document.querySelector("input");
input?.addEventListener("input", () => {
    if (input.value.includes(" ")) {
        input.setCustomValidity("Cannot contain spaces");
    } else {
        input.setCustomValidity("");
    }
});
```

If the message is an empty string, the element is considered to be
valid.

#### validationMessage

The `validationMessage` property contains a localized message describing
the validation constraints that the control doesn't satisfy (if any). If
the control value is valid, this will produce an empty string.

```ts
const input = document.querySelector("input");
input?.addEventListener("invalid", () => {
    alert(input.validationMessage);
});
```
