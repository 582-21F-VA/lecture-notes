# JavaScript

**JavaScript** was introduced in 1995 as a way to add programs to web
pages in the Netscape Navigator browser. The language has since been
adopted by all other major graphical web browsers as a tool for adding
various forms of interactivity and cleverness. Today still, JavaScript
is the only programming language that can be interpreted by the browser.

It is important to note that JavaScript has almost nothing to do with
the programming language named **Java**. The similar name was inspired
by marketing considerations rather than good judgment. When JavaScript
was being introduced, the Java language was being heavily marketed and
was gaining popularity. Someone thought it was a good idea to try to
ride along on this success. Now we are stuck with the name.

After its adoption outside of Netscape, a standard document was written
to describe the way the JavaScript language should work so that the
various pieces of software that claimed to support JavaScript could make
sure they actually provided the same language. This is called the
**ECMAScript** standard, after the Ecma International organization that
conducted the standardization. In practice, the terms ECMAScript and
JavaScript can be used interchangeably — they are two names for the same
language.

## Runtime

Nowadays, JavaScript can also be used _outside_ the web browser.
**Runtime environments** such as [Node], [Deno] and [Bun] allow you to
execute JavaScript programs from the command-line, without having to
rely on the browser. They make it possible to write servers and
applications such as [Visual Studio Code], [Obsidian] and [Discord]
entirely in JavaScript.

This course being about client-side programming, most of our programs
will be run in the web browser. That being said, the way JavaScript
interacts with a web page is complex. Before addressing this topic, we
will begin by familiarizing ourselves with the language in a simpler
environment. To do this, we will use [Bun].

[Node]: https://nodejs.org/en
[Deno]: https://deno.com/
[Bun]: https://bun.com/
[Visual Studio Code]: https://code.visualstudio.com/
[Obsidian]: https://obsidian.md/
[Discord]: https://discord.com/

### Installing Bun

To install Bun, we recommend using a package manager such as [Scoop] for
Windows and [Homebrew] for Mac. For Linux, it depends on your
distribution. Once you have installed a package manager, the following
commands can be used to install Bun.

Windows:

```sh
scoop install bun
```

Mac:

```sh
brew install oven-sh/bun/bun
```

[Scoop]: https://scoop.sh
[Homebrew]: https://brew.sh

### Using Bun

In this course, we will use Bun to create projects, execute programs,
run the development server, lint and format code, and submit
assignments.

To create a new project for an homework assignment or an exercise, run
the following command, replacing `<name>` with the name of the
assignment:

```sh
bun create 582-21F-VA/starter <name>
```

This will create a directory with all the necessary configuration files.
You can then open your project with the command `cd <name>`, and run
your code with `bun index.ts`.

## Resources

[MDN Web Docs] is the primary documentation source for the JavaScript
language. It includes [beginner tutorials], [guides], and
[reference material]. [The Modern JavaScript Tutorial] is also an
excellent resource, as is the book [JavaScript: The Definitive Guide] by
David Flanagan. We recommend avoiding W3Schools, as it contains
erroneous information and promotes poor practices.

[MDN Web Docs]: https://developer.mozilla.org/en-US/
[beginner tutorials]: https://developer.mozilla.org/en-US/docs/Web/JavaScript#beginners_tutorials
[guides]: https://developer.mozilla.org/en-US/docs/Web/JavaScript#javascript_guides
[reference material]: https://developer.mozilla.org/en-US/docs/Web/JavaScript#reference
[The Modern JavaScript Tutorial]: https://javascript.info
[JavaScript: The Definitive Guide]: https://www.oreilly.com/library/view/javascript-the-definitive/9781491952016/
