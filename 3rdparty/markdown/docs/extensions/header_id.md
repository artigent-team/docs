title: HeaderId Extension

HeaderId
========

Summary
-------

The HeaderId extension automatically generates `id` attributes for the header
elements (`h1`-`h6`) in the resulting HTML document.

This extension is included in the standard Markdown library.

!!! warning
    This extension is **Pending Deprecation**. The [Table of Contents][toc]
    Extension should be used instead, which offers most the features of this
    extension and more.

[toc]: toc.md

Syntax
------

By default, all headers will automatically have unique `id` attributes
generated based upon the text of the header (see below to turn this off).
Note this example, in which all three headers would have the same `id`:

```md
#Header
#Header
#Header
```

Results in:

```html
<h1 id="header">Header</h1>
<h1 id="header_1">Header</h1>
<h1 id="header_2">Header</h1>
```

Usage
-----

See [Extensions](index.md) for general extension usage, specify
`markdown.extensions.headerid` as the name of the extension.

See the [Library Reference](../reference.md#extensions) for information about
configuring extensions.

The following options are provided to configure the output:

* **`level`**: Base level for headers.

    Default: `1`

    The `level` setting allows you to automatically adjust the header levels to
    fit within the hierarchy of your HTML templates. For example, suppose the
    markdown text for a page should not contain any headers higher than level 3
    (`<h3>`). The following will accomplish that:

        :::pycon
        >>>  text = '''
        ... #Some Header
        ... ## Next Level'''
        >>> from markdown.extensions.headerid import HeaderIdExtension
        >>> html = markdown.markdown(text, extensions=[HeaderIdExtension(level=3)])
        >>> print html
        <h3 id="some_header">Some Header</h3>
        <h4 id="next_level">Next Level</h4>'

* **`forceid`**: Force all headers to have an id.

    Default: `True`

    The `forceid` setting turns on or off the automatically generated ids for
    headers that do not have one explicitly defined (using the
    [Attribute List](attr_list.md) extension).

        :::pycon
        >>> text = '''
        ... # Some Header
        ... # Header with ID # { #foo }'''
        >>> html = markdown.markdown(text,
                                     extensions=['markdown.extensions.attr_list',
                                                 HeaderIdExtension(forceid=False)])
        >>> print html
        <h1>Some Header</h1>
        <h1 id="foo">Header with ID</h1>

* **`separator`**: Word separator. Character which replaces white space in id.

    Default: `-`

* **`slugify`**: Callable to generate anchors.

    Default: `markdown.extensions.headerid.slugify`

    If you would like to use a different algorithm to define the ids, you can
    pass in a callable which takes two arguments:

    * `value`: The string to slugify.
    * `separator`: The Word Separator.

Using with Meta-Data
--------------------

The HeaderId extension also supports the [Meta-Data](meta_data.md) extension.
Please see the documentation for that extension for specifics. The supported
meta-data keywords are:

* `header_level`
* `header_forceid`

When used, the meta-data will override the settings provided through the
`extension_configs` interface.

This document:

```md
header_level: 2
header_forceid: Off

# A Header
```

Will result in the following output:

```html
<h2>A Header</h2>
```
