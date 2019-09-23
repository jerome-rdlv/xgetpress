# xgetpress

This command line utility tries to reproduce the WordPress support of [Poedit Pro][poedit]:

* String extraction from sources based on `X-Poedit-` settings
* Plugin or theme metadata extraction from WordPress file headers
* Input file update with new string

It takes a `.pot` or `.po` file as only argument:

```bash
xgetpress theme.pot
```

A colored `diff` is displayed at the end of the process so you can
see what has changed in your input file.

As for Poedit, gettext tools are used for underlying tasks:

* `xgettext` for string extraction
* `msguniq` for duplicate strings merge and resolution
* `msgmerge` for input file update

I wrote that thing mainly because I needed a CLI utility
that Poedit does not offer.

**This is really fresh and there may be bugs, and possible
discrepancies with Poedit behavior. Use at your own risks.**

That said, I’d be really glad to be notified of those, and I
will try to correct them as far as possible.

[poedit]: https://poedit.net/pro
