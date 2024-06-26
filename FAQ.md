
# Frequently Asked Questions
- [How do I log a renderable?](#how-do-i-log-a-renderable)
- [How do I render console markup in RichHandler?](#how-do-i-render-console-markup-in-richhandler)
- [Natively inserted ANSI escape sequence characters break alignment of Panel.](#natively-inserted-ansi-escape-sequence-characters-break-alignment-of-panel)
- [python -m rich.spinner shows extra lines.](#python--m-richspinner-shows-extra-lines)
- [Rich is automatically installing traceback handler.](#rich-is-automatically-installing-traceback-handler)
- [Strange colors in console output.](#strange-colors-in-console-output)
- [Why does content in square brackets disappear?](#why-does-content-in-square-brackets-disappear)
- [Why does emoji break alignment in a Table or Panel?](#why-does-emoji-break-alignment-in-a-table-or-panel)

<a name="how-do-i-log-a-renderable"></a>
## How do I log a renderable?

Python's logging module is designed to work with strings. Consequently you won't be able to log Rich renderables (Table, Tree, etc) by calling `logger.debug` or other similar method.

You could use the [capture](https://rich.readthedocs.io/en/latest/console.html#capturing-output) API to convert the renderable to a string and log that. However I would advise against it.

Logging supports configurable back-ends, which means that a log message could go somewhere other than the terminal -- which may not correctly render the formatting and style produced by Rich.

If you are only logging with a file-handler to stdout, then you probably don't need to use the logging module at all. Consider using [Console.log](https://rich.readthedocs.io/en/latest/reference/console.html#rich.console.Console.log) which will render anything that you can print with Rich, with a timestamp.

<a name="how-do-i-render-console-markup-in-richhandler"></a>
## How do I render console markup in RichHandler?

Console markup won't work anywhere else, other than `RichHandler` -- which is why they are disabled by default.

See the docs if you want to [enable console markup](https://rich.readthedocs.io/en/latest/logging.html#logging-handler) in the logging handler.

<a name="natively-inserted-ansi-escape-sequence-characters-break-alignment-of-panel"></a>
## Natively inserted ANSI escape sequence characters break alignment of Panel.

If you print ansi escape sequences for color and style you may find the output breaks your output.
You may find that border characters in Panel and Table are in the wrong place, for example.

As a general rule, you should allow Rich to generate all ansi escape sequences, so it can correctly account for these invisible characters.
If you can't avoid a string with escape codes, you can convert it to an equivalent `Text` instance with `Text.from_ansi`.

<a name="python--m-richspinner-shows-extra-lines"></a>
## python -m rich.spinner shows extra lines.

The spinner example is know to break on some terminals (Windows in particular).

Some terminals don't display emoji with the correct width, which means Rich can't always align them accurately inside a panel.

<a name="rich-is-automatically-installing-traceback-handler"></a>
## Rich is automatically installing traceback handler.

Rich will never install the traceback handler automatically.

If you are getting Rich tracebacks and you don't want them, then some other piece of software is calling `rich.traceback.install()`.

<a name="strange-colors-in-console-output"></a>
## Strange colors in console output.

Rich will highlight certain patterns in your output such as numbers, strings, and other objects like IP addresses.

Occasionally this may also highlight parts of your output you didn't intend. See the [docs on highlighting](https://rich.readthedocs.io/en/latest/highlighting.html) for how to disable highlighting.

<a name="why-does-content-in-square-brackets-disappear"></a>
## Why does content in square brackets disappear?

Rich will treat text within square brackets as *markup tags*, for instance `"[bold]This is bold[/bold]"`.

If you are printing strings with literally square brackets you can either disable markup, or escape your strings.
See the docs on [console markup](https://rich.readthedocs.io/en/latest/markup.html) for how to do this.

<a name="why-does-emoji-break-alignment-in-a-table-or-panel"></a>
## Why does emoji break alignment in a Table or Panel?

Certain emoji take up double space within the terminal. Unfortunately, terminals don't always agree how wide a given character should be.

Rich has no way of knowing how wide a character will be on any given terminal. This can break alignment in containers like Table and Panel, where Rich needs to know the width of the content.

There are also *multiple codepoints* characters, such as country flags, and emoji modifiers, which produce wildly different results across terminal emulators.

Fortunately, most characters will work just fine. But you may have to avoid using the emojis that break alignment. You will get good results if you stick to emoji released on or before version 9 of the Unicode database,

<hr>

Generated by [FAQtory](https://github.com/willmcgugan/faqtory)
