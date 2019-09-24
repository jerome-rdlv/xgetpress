# xgetpress

This command line utility tries to reproduce the WordPress support of [Poedit Pro][poedit]:

* String extraction from sources based on `X-Poedit-` settings
* Plugin or theme metadata extraction from WordPress file headers
* Input file update with new string

It takes a `.pot` or `.po` file as only argument:

```bash
xgetpress theme.pot
```

A colored `diff` is displayed at the end of the process so you can see what has changed in your input file.

As for Poedit, gettext tools are used for underlying tasks:

* `xgettext` for string extraction
* `msguniq` for duplicate strings merge and resolution
* `msgmerge` for input file update

I wrote that thing mainly because I needed a CLI utility that Poedit does not offer.

**This is really fresh and there may be bugs, and possible discrepancies with Poedit behavior. Use at your own risks.**

That said, I’d be really glad to be notified of those, and I will try to correct them as far as possible.

This tool currently extract only from PHP files. Is there situations where other languages like JS should be supported? You’ll tell me.

## Possible headers

Here is a list of the extended headers you can use in your `.po` or `.pot` files that will be interpreted by `xgetpress`:

| Headers                           | Description     |
|-----------------------------------|:----------------|
| `X-Poedit-KeywordsList`           | List of the keywords to look for during extraction, separated by semicolon (;). By default `__` and `_e`. Each of these is passed to `xgettext` with option `-k`.
| `X-Poedit-Basepath`               | Base path of your project, relative to the `.po` or `.pot` input file.
| `X-Poedit-SearchPath-*`           | Paths to search for translatable strings. The `*` must be replaced with an incrementing number, so for two search paths you would have `X-Poedit-SearchPath-0` and `X-Poedit-SearchPath-1`. These paths are relative to the base path.
| `X-Poedit-SearchPathExcluded-*`   | Same as above but to exclude paths. This is the main point of possible discrepancies with Peodit. `xgetpress` behavior is to match patterns (string containing a `*`) at the end of the paths, and all other strings at the beginning of the path. It seems to be the behavior of Poedit, but that’s rather an expeditious investigation.
| `X-Poedit-WPHeader`               | Path to the file containing the WordPress headers, either the `style.css` file of a theme, or the `plugin-name.php` file of a plugin.
| `X-Poedit-SourceCharset`          | Charset of your sources. Defaults to `UTF-8`
| `X-Poedit-Flags-xgettext`         | Options to directly pass to `xgettext`, for example `--add-comments=L10N:`

## Example file

Beginning of an example `.pot` file:

```pot
msgid ""
msgstr ""
"Project-Id-Version: Your project name and version\n"
"POT-Creation-Date: 2019-09-24 21:44+0200\n"
"Last-Translator:  Your name <your-email@example.com>\n"
"MIME-Version: 1.0\n"
"Content-Type: text/plain; charset=UTF-8\n"
"Content-Transfer-Encoding: 8bit\n"
"X-Poedit-KeywordsList: __;_e;_n:1,2;_x:1,2c;_ex:1,2c;_nx:4c,1,2;esc_attr__;"
"esc_attr_e;esc_attr_x:1,2c;esc_html__;esc_html_e;esc_html_x:1,2c;_n_noop:1,2;"
"_nx_noop:3c,1,2;__ngettext_noop:1,2\n"
"X-Poedit-SourceCharset: UTF-8\n"
"X-Poedit-Basepath: ..\n"
"X-Poedit-WPHeader: style.css\n"
"X-Poedit-SearchPath-0: .\n"
"X-Poedit-SearchPathExcluded-0: some-dir\n"
"X-Poedit-Flags-xgettext: --add-comments=L10N:\n"
```

Corresponding `style.css` example:

```css
/*
 * Theme Name: Your project name
 * Theme URI: https://example.org/
 * Author: Your name
 * Author URI: https://example.org/author
 */
```

With the given example, your theme name, URI, author name and author URI will be extracted as translatable string and be added to the `.pot` file.

```POT
#. Author of the theme
#: style.css
msgid "Your name"
msgstr ""

#. Name of the theme
#: style.css
msgid "Your project name"
msgstr ""

#. URI of the theme
#: style.css
msgid "https://example.org/"
msgstr ""

#. Author URI of the theme
#: style.css
msgid "https://example.org/author"
msgstr ""
```


[poedit]: https://poedit.net/pro
