# JavaScript and the browser

JavaScript was created to be run in web browsers. Nothing is simpler
than to execute JavaScript in an HTML document. You need only to wrap
the code in `<script>` tags, and voilà !

```html
<h1>Testing alert</h1>
<script>
    alert("hello!");
</script>
```

Such a script will run as soon as its `<script>` tag is encountered
while the browser reads the HTML. This page will pop up a dialog when
opened; the `alert` function resembles `prompt`, in that it pops up a
little window, but only shows a message without asking for input.

## Modules

Including large programs directly in HTML documents, however, is
impractical, and so you should avoid it. Instead, the `<script>` tag can
be given a `src` attribute to fetch a text file containing JavaScript
from a URL. When an HTML page references other URLs as part of itself,
such as an image file or a script, web browsers will retrieve them
immediately and include them in the page.

```html
<head>
    <script src="/hello.js" type="module"></script>
</head>
```

Scripts of type "module" are special in that they can use `import`
statements to use bindings defined in other modules. Browsers also
**defer** the execution of modules until after the entire HTML document
as been parsed, so you can put the script tag in the `<head>` where it
belongs. For these reasons, always give your script tags the attribute
`type="module"`.

Most browsers will block access to local JavaScript modules if they are
not served by a server. Luckily, most programming languages come with
built-in file servers that you can use to serve static ressources. With
Bun, for instance, you can run the `bun index.html` command in your
terminal to launch a simple web server. If an "index.html" file is
present in the current working directory, you will be able to see it in
your browser at the `localhost:3000` address. Press CTRL + C in your
terminal to stop the server.
