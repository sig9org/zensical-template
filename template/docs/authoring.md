---
title: Authoring
icon: simple/markdown
---

## Basics

A Zensical project is configured via a `zensical.toml` file. If you create your
project using the [`new` command][new], this file will be automatically created
for you, and include an example configuration with comments describing the
available settings.

[new]: ../usage/new.md

??? info "Why Zensical uses TOML"

    The [TOML file format] is specifically designed to be easy to scan and
    understand. We've chosen TOML over YAML, since it avoids a number of
    problems that YAML suffers from:

    * YAML uses indentation to express structure, which makes it particularly
      error prone to indentation mistakes that are hard to locate. In TOML,
      whitespace is mostly a stylistic choice.

    * In YAML, values do not need to be escaped, which can cause ambiguities if
      a value can be interpreted as different types, such as `no`, which could
      the a string or a boolean. TOML requires all strings to be quoted.

### Transition from MkDocs

To ease transition from [Material for MkDocs], Zensical can natively read
`mkdocs.yml` configuration files. However, we recommend that new projects use
`zensical.toml` files. This documentation lists configuration options for
both configuration file formats in content tabs.

???+ note "Why we support `mkdocs.yml`"

    Since Zensical is built by the creators of Material for MkDocs, we support
    configuration via `mkdocs.yml` files as a transition mechanism to make
    migration to Zensical smooth for users that have existing projects. Support
    for configuration with `mkdocs.yml` will always be supported, but
    eventually move out of the core.

[Material for MkDocs]: https://squidfunk.github.io/mkdocs-material/

### The `project` scope

A `zensical.toml` configuration begins with a line declaring a scope for the
project:

``` toml
[project]
```

As of now, all settings are contained within this scope. As we evolve Zensical,
we will introduce additional scopes and move settings out of the `project`
scope where appropriate. Of course, we'll provide automatic refactorings, so
there's no need for manual migration.

### Theme variant

Zensical comes with two theme variants: `modern` and `classic`, with `modern`
being the default – you're looking at it right now. The `classic` theme exactly
matches the look and feel of Material for MkDocs, while the `modern` theme
provides a fresh new design.

In case you're coming from [Material for MkDocs] and want to keep its look and
feel, or customize your site based on its appearance, you can switch to the
`classic` theme variant:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    variant = "classic"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      variant: classic
    ```

!!! note "Customization"

    The HTML structure of Zensical matches that of Material for MkDocs in both
    theme variants. This means that your existing customizations with CSS and
    JavaScript should work with either theme variant. However, there may be
    circumstances where customizations do not yield the desired results.

    In that case we recommend switching to the `classic` variant.

### Settings

#### `site_name`

The `site_name` is a required setting that provides the name of the site to be
included in the HTML head and in the page headers.

=== "`zensical.toml`"

    ``` toml
    [project]
    site_name = "My Zensical project"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    site_name: My Zensical project
    ```

#### `site_url`

The `site_url` specifies the canonical URL for the site, which appears in the
HTML header and should be set unless you're building for [offline usage].

=== "`zensical.toml`"

    ``` toml
    [project]
    site_url = "https://example.com"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    site_url: https://example.com
    ```

  [offline usage]: offline.md

#### `site_description`

A `site_description` is used in the HTML head if the page itself does not
specify a [description in the page metadata]. Some search engines use this
to describe the page content.

=== "`zensical.toml`"

    ``` toml
    [project]
    site_description = "Lorem ipsum dolor sit amet, consectetur adipiscing elit."
    ```

=== "`mkdocs.yml`"

    ``` yaml
    site_description: Lorem ipsum dolor sit amet, consectetur adipiscing elit.
    ```

[description in the page metadata]: ../authoring/frontmatter.md

#### `site_author`

The `site_author` setting is used in the HTML `head` element  to indicate the
author of a website.

=== "`zensical.toml`"

    ``` toml
    [project]
    site_author = "John Doe"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    site_author: John Doe
    ```

#### `copyright`

The `copyright` setting allows you to specify a copyright notice that will be
inserted into the footer of your pages. You can specify an HTML fragment here or
just plain text.

=== "`zensical.toml`"

    ``` toml
    [project]
    copyright = "&copy; 2025 Jane Doe"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    copyright: "&copy; 2025 Jane Doe"
    ```

#### `docs_dir`

The `docs_dir` setting specifies the path to the directory that contains your
source files. This must be a relative path, which is resolved relative to the
configuration file.

=== "`zensical.toml`"

    ``` toml
    [project]
    docs_dir = "docs"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    docs_dir: docs
    ```

#### `site_dir`

The `site_dir` specifies the path to the directory your site will be written to.
This must be a relative path, which is resolved relative to the configuration
file.

=== "`zensical.toml`"

    ``` toml
    [project]
    site_dir = "site"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    site_dir: site
    ```

#### `extra`

The `extra` configuration option serves as a way to store arbitrary key-value
pairs that are used by templates. If you override templates, you can use these
values to customize behavior.

=== "`zensical.toml`"

    ``` toml
    [project.extra]
    key = "value"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra:
      key: value
    ```

#### `use_directory_urls`

The `use_directory_urls` setting controls the directory structure of your
documentation site, and thereby the URL format used for linking to pages.

=== "`zensical.toml`"

    ``` toml
    [project]
    use_directory_urls = false
    ```

=== "`mkdocs.yml`"

    ``` yaml
    use_directory_urls: false
    ```

Note that this is automatically set to `false` when building for [offline usage],
so your documentation can be browsed from a local filesystem without a web
server. The default value is `true`.

=== "`true`"

    Source file      | Generated File     | URL Format
    ---------------- | ------------------ | -------------------
    index.md         | index.html         | /
    usage.md         | usage.html         | /usage/
    about/license.md | about/license.html | /about/license/

=== "`false`"

    Source file      | Generated File     | URL Format
    ---------------- | ------------------ | -------------------
    index.md         | index.html         | /index.html
    usage.md         | usage.html         | /usage.html
    about/license.md | about/license.html | /about/license.html

#### `dev_addr`

When running `zensical serve`, the built-in web server binds to this address
to serve your documentation site locally. Note that you need to specify an IP
address and a port.

=== "`zensical.toml`"

    ``` toml
    [project]
    dev_addr = "localhost:3000"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    dev_addr: localhost:3000
    ```

The default `dev_addr` is `localhost:8000`.

### Unsupported settings

The following `mkdocs.yml` settings are not (yet) supported in Zensical, as
we're rethinking how configuration and customization should work:

- `remote_branch`
- `remote_name`
- `exclude_docs`
- `draft_docs`
- `not_in_nav`
- `validation`
- `strict`
- `hooks`
- `watch`

## Colors

Zensical allows to change the color palette of your documentation site through
configuration to fit your brand's identity. If you want to go beyond that, you
can also define [custom colors].

  [custom colors]: #custom-colors

### Configuration

#### Color palette

##### Color scheme

Zensical supports two color schemes: a __light mode__, which is called `default`,
and a __dark mode__, which is called `slate`. The color scheme can be set via
configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme.palette]
    scheme = "default"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      palette:
        scheme: default
    ```

Click on a tile to preview the color scheme:

<div class="mdx-switch">
  <button data-md-color-scheme-2="default"><code>default</code></button>
  <button data-md-color-scheme-2="slate"><code>slate</code></button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-scheme-2]")
  buttons.forEach(function(button) {
    button.addEventListener("click", function() {
      document.body.setAttribute("data-md-color-switching", "")
      var attr = this.getAttribute("data-md-color-scheme-2")
      document.body.setAttribute("data-md-color-scheme", attr)
      setTimeout(function() {
        document.body.removeAttribute("data-md-color-switching")
      })
    })
  })
</script>

##### Primary color

The primary color is used for text links, and in the `classic` theme, for
the header and the sidebar. In order to change the primary color, set the
setting to a valid color name:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    palette.primary = "indigo"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      palette:
        primary: indigo
    ```

Click on a tile to preview the primary color:

<div class="mdx-switch">
  <button data-md-color-primary="red"><code>red</code></button>
  <button data-md-color-primary="pink"><code>pink</code></button>
  <button data-md-color-primary="purple"><code>purple</code></button>
  <button data-md-color-primary="deep-purple"><code>deep purple</code></button>
  <button data-md-color-primary="indigo"><code>indigo</code></button>
  <button data-md-color-primary="blue"><code>blue</code></button>
  <button data-md-color-primary="light-blue"><code>light blue</code></button>
  <button data-md-color-primary="cyan"><code>cyan</code></button>
  <button data-md-color-primary="teal"><code>teal</code></button>
  <button data-md-color-primary="green"><code>green</code></button>
  <button data-md-color-primary="light-green"><code>light green</code></button>
  <button data-md-color-primary="lime"><code>lime</code></button>
  <button data-md-color-primary="yellow"><code>yellow</code></button>
  <button data-md-color-primary="amber"><code>amber</code></button>
  <button data-md-color-primary="orange"><code>orange</code></button>
  <button data-md-color-primary="deep-orange"><code>deep orange</code></button>
  <button data-md-color-primary="brown"><code>brown</code></button>
  <button data-md-color-primary="grey"><code>grey</code></button>
  <button data-md-color-primary="blue-grey"><code>blue grey</code></button>
  <button data-md-color-primary="black"><code>black</code></button>
  <button data-md-color-primary="white"><code>white</code></button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-primary]")
  buttons.forEach(function(button) {
    button.addEventListener("click", function() {
      var attr = this.getAttribute("data-md-color-primary")
      document.body.setAttribute("data-md-color-primary", attr)
    })
  })
</script>

See our guide below to learn how to set [custom colors].

##### Accent color

The accent color is used to denote elements that can be interacted with, e.g.
hovered links, buttons, and scrollbars. It can be changed in your configuration
by choosing a valid color name:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    palette.accent = "indigo"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      palette:
        accent: indigo
    ```

Click on a tile to preview the accent color:

<div class="mdx-switch">
  <button data-md-color-accent="red"><code>red</code></button>
  <button data-md-color-accent="pink"><code>pink</code></button>
  <button data-md-color-accent="purple"><code>purple</code></button>
  <button data-md-color-accent="deep-purple"><code>deep purple</code></button>
  <button data-md-color-accent="indigo"><code>indigo</code></button>
  <button data-md-color-accent="blue"><code>blue</code></button>
  <button data-md-color-accent="light-blue"><code>light blue</code></button>
  <button data-md-color-accent="cyan"><code>cyan</code></button>
  <button data-md-color-accent="teal"><code>teal</code></button>
  <button data-md-color-accent="green"><code>green</code></button>
  <button data-md-color-accent="light-green"><code>light green</code></button>
  <button data-md-color-accent="lime"><code>lime</code></button>
  <button data-md-color-accent="yellow"><code>yellow</code></button>
  <button data-md-color-accent="amber"><code>amber</code></button>
  <button data-md-color-accent="orange"><code>orange</code></button>
  <button data-md-color-accent="deep-orange"><code>deep orange</code></button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-accent]")
  buttons.forEach(function(button) {
    button.addEventListener("click", function() {
      var attr = this.getAttribute("data-md-color-accent")
      document.body.setAttribute("data-md-color-accent", attr)
    })
  })
</script>

See our guide below to learn how to set [custom colors].

#### Color palette toggle

Offering a light and dark color palette makes your documentation pleasant to
read at different times of the day, so the user can choose accordingly. Add the
following lines to allow users to switch between light and dark mode:

=== "`zensical.toml`"

    ``` toml
    [[project.theme.palette]] # (1)!
    scheme = "default"
    toggle.icon = "lucide/sun"
    toggle.name = "Switch to dark mode"

    [[project.theme.palette]]
    scheme = "slate"
    toggle.icon = "lucide/moon"
    toggle.name = "Switch to light mode"
    ```

    1.  Note that the `theme.palette` setting is defined as a list.

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      palette: # (1)!

        # Palette toggle for light mode
        - scheme: default
          toggle:
            icon: lucide/sun
            name: Switch to dark mode

        # Palette toggle for dark mode
        - scheme: slate
          toggle:
            icon: lucide/moon
            name: Switch to light mode
    ```

    1.  Note that the `theme.palette` setting is now defined as a list.

You can use any icon from an [available icon set] for the toggle icon.

  [available icon set]: ../authoring/icons-emojis.md#included-icon-sets

This configuration will render a color palette toggle next to the search bar.
Note that you can also define separate settings for [`primary`][palette.primary]
and [`accent`][palette.accent] per color palette.

The following properties must be set for each toggle:

`icon`

:    This property must point to a valid icon path referencing any icon
bundled with the theme, or the build will not succeed. Some popular combinations
in addition to the ones above:

    * :lucide-sun: + :lucide-moon: – `lucide/sun` + `lucide/moon`
    * :lucide-toggle-left: + :lucide-toggle-right: – `lucide/toggle-left` + `lucide/toggle-right`

`name`
:   This property is used as the toggle's `title` attribute and should be set to
    a discernable name to improve accessibility. It's rendered as a [tooltip].

  [palette.scheme]: #color-scheme
  [palette.primary]: #primary-color
  [palette.accent]: #accent-color
  [tooltip]: ../authoring/tooltips.md

#### System preference

Each color palette can be linked to the user's system preference for light and
dark appearance by using a media query. Simply add a `media` property next to
the `scheme` setting in the configuration:

=== "`zensical.toml`"

    ``` toml
    # Palette toggle for light mode
    [[project.theme.palette]]
    media = "(prefers-color-scheme: light)"
    scheme = "default"
    toggle.icon = "lucide/sun"
    toggle.name = "Switch to dark mode"

    # Palette toggle for dark mode
    [[project.theme.palette]]
    media = "(prefers-color-scheme: dark)"
    scheme = "slate"
    toggle.icon = "lucide/moon"
    toggle.name = "Switch to light mode"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      palette:

        # Palette toggle for light mode
        - media: "(prefers-color-scheme: light)"
          scheme: default
          toggle:
            icon: lucide/sun
            name: Switch to dark mode

        # Palette toggle for dark mode
        - media: "(prefers-color-scheme: dark)"
          scheme: slate
          toggle:
            icon: lucide/moon
            name: Switch to light mode
    ```

When the user first visits your site, the media queries are evaluated in the
order of their definition. The first media query that matches selects the
default color palette.

##### Automatic light / dark mode

Zensical has support for automatic light / dark mode, delegating color palette
selection to the user's operating system. Add the following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    # Palette toggle for automatic mode
    [[project.theme.palette]]
    media = "(prefers-color-scheme)"
    toggle.icon = "lucide/sun-moon"
    toggle.name = "Switch to light mode"

    # Palette toggle for light mode
    [[project.theme.palette]]
    media = "(prefers-color-scheme: light)"
    scheme = "default"
    toggle.icon = "lucide/sun"
    toggle.name = "Switch to dark mode"

    # Palette toggle for dark mode
    [[project.theme.palette]]
    media = "(prefers-color-scheme: dark)"
    scheme = "slate"
    toggle.icon = "lucide/moon"
    toggle.name = "Switch to system preference"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      palette:

        # Palette toggle for automatic mode
        - media: "(prefers-color-scheme)"
          toggle:
            icon: lucide/sun-moon
            name: Switch to light mode

        # Palette toggle for light mode
        - media: "(prefers-color-scheme: light)"
          scheme: default # (1)!
          toggle:
            icon: lucide/sun
            name: Switch to dark mode

        # Palette toggle for dark mode
        - media: "(prefers-color-scheme: dark)"
          scheme: slate
          toggle:
            icon: lucide/moon
            name: Switch to system preference
    ```

    1.  You can also define separate settings for [`primary`][palette.primary] and
        [`accent`][palette.accent] per color palette, i.e. different colors for
        light and dark mode.

Zensical will now change the color palette each time the operating
system switches between light and dark appearance, even when the user doesn't
reload the site.

### Customization

#### Custom colors

Zensical implements colors using [CSS variables] (custom properties). If you
want to customize the colors beyond the palette (e.g. to use your brand-specific
colors), you can add an [additional style sheet] and tweak the values of the CSS
variables.

First, set the [`primary`][palette.primary] or [`accent`][palette.accent] values
in `mkdocs.yml` to `custom`, to signal to the theme that you want to define
custom colors, e.g., when you want to override the `primary` color:

=== "`zensical.toml`"

    ``` toml
    [project.theme.palette]
    primary = "custom"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      palette:
        primary: custom
    ```

Let's say you're :fontawesome-brands-youtube:{ style="color: #EE0F0F" }
__YouTube__, and want to set the primary color to your brand's palette. Just
add this CSS and make sure that it is included in the `etxra_css` setting in
your configuration:

=== "`docs/stylesheets/extra.css`"

    ``` css
    :root  > * {
      --md-primary-fg-color:        #EE0F0F;
      --md-primary-fg-color--light: #ECB7B7;
      --md-primary-fg-color--dark:  #90030C;
    }
    ```

=== "`zensical.toml`"

    ``` toml
    [project]
    extra_css = ["stylesheets/extra.css"]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra_css:
      - stylesheets/extra.css
    ```

  [CSS variables]: https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties

#### Custom color schemes

Besides overriding specific colors, you can create your own, named color scheme
by wrapping the definitions in a `[data-md-color-scheme="..."]`
[attribute selector], which you can then set via your configuration as described
in the [color schemes][palette.scheme] section:

=== "`docs/stylesheets/extra.css`"

    ``` css
    [data-md-color-scheme="youtube"] {
      --md-primary-fg-color:        #EE0F0F;
      --md-primary-fg-color--light: #ECB7B7;
      --md-primary-fg-color--dark:  #90030C;
    }
    ```

=== "`zensical.toml`"

    ``` toml
    [project]
    theme.palette.scheme = "youtube"
    extra_css = ["stylesheets/extra.css"]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      palette:
        scheme: youtube
    extra_css:
      - stylesheets/extra.css
    ```

Additionally, the `slate` color scheme defines all of it's colors via `hsla`
color functions and deduces its colors from the `--md-hue` CSS variable. You
can tune the `slate` theme with:

``` css
[data-md-color-scheme="slate"] {
  --md-hue: 210; /* (1)! */
}
```

1. The `hue` value must be in the range of `[0, 360]`

  [attribute selector]: https://www.w3.org/TR/selectors-4/#attribute-selectors

## Fonts

Zensical makes it easy to change the typeface of your project documentation,
since it directly integrates with [Google Fonts]. Alternatively, fonts can be
custom-loaded if self-hosting is preferred for data privacy reasons or if
another destination should be used.

  [Google Fonts]: https://fonts.google.com

### Configuration

#### Regular font

The regular font is used for all body copy, headlines, and essentially
everything that does not need to be monospaced. It can be set to any
valid [Google Font][Google Fonts] via your configuration:

=== "`zensical.toml`"
    ``` toml
    [project.theme]
    font.text = "Inter"
    ```

=== "`mkdocs.yml`"
    ``` yaml
    theme:
      font:
        text: Inter
    ```

#### Monospaced font

The _monospaced font_ is used for code blocks and can be configured separately.
Just like the regular font, it can be set to any valid [Google Font]
[Google Fonts] via your configuration:

=== "`zensical.toml`"
    ``` toml
    [project.theme]
    font.code = "JetBrains Mono"
    ```

=== "`mkdocs.yml`"
    ``` yaml
    theme:
      font:
        code: JetBrains Mono
    ```

#### Autoloading

If you want to prevent typefaces from being loaded from [Google Fonts], e.g.
to adhere to [data privacy] regulations, and fall back to system fonts, add the
following lines to your configuration:

=== "`zensical.toml`"
    ``` toml
    [project.theme]
    font = false
    ```

=== "`mkdocs.yml`"
    ``` yaml
    theme:
      font: false
    ```

  [data privacy]: https://developers.google.com/fonts/faq/privacy

### Customization

#### Additional fonts

If you want to load an (additional) font from another destination or override
the system font, you can use an [additional style sheet] to add the
corresponding `@font-face` definition:

=== "`docs/stylesheets/extra.css`"

    ``` css
    @font-face {
      font-family: "<font>";
      src: "...";
    }
    ```

=== "`zensical.toml`"
    ``` toml
    [project]
    extra_css = ["stylesheets/extra.css"]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra_css:
      - stylesheets/extra.css
    ```

The font can then be applied to specific elements, e.g. only headlines, or
globally to be used as the site-wide regular or monospaced font:

=== "Regular font"

    ``` css
    :root {
      --md-text-font: "<font>"; /* (1)! */
    }
    ```

    1.  Always define fonts through CSS variables and not `font-family`, as
        this would disable the system font fallback.

=== "Monospaced font"

    ``` css
    :root {
      --md-code-font: "<font>";
    }
    ```

  [additional style sheet]: ../customization.md#additional-css

## Language

With the help of contributors, templates are localized into more than 60
languages, making it easy to create documentation sites in your preferred
language.

### Configuration

#### Site language

You can set the site language in your configuration file with:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    language = "en"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      language: en
    ```

HTML5 only allows to set a [single language per document], which is why Zensical
only supports setting a canonical language for the entire project.

The following languages are supported:

--8<-- "includes/languages.html"

  [single language per document]: https://www.w3.org/International/questions/qa-html-language-declarations.en#attributes

#### Site language selector

If your documentation is available in multiple languages, a language selector
pointing to those languages can be added to the header. Alternate languages
can be defined via configuration:

=== "`zensical.toml`"

    ``` toml
    [project.extra]
    alternate = [
        { name = "English", link = "/en/", lang = "en" },
        { name = "Deutsch", link = "/de/", lang = "de" }
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra:
      alternate:
        - name: English
          link: /en/ # (1)!
          lang: en
        - name: Deutsch
          link: /de/
          lang: de
    ```

The following properties are required for each alternate language:

`alternate.name`

:   This value of this property is used inside the language selector as the
    name of the language and must be set to a non-empty string.

`alternate.link`

:    This property must be set to an absolute link, which might also point to
     another domain or subdomain not necessarily generated with Zensical.
     If it includes a domain part, it's used as defined. Otherwise the domain
     part of the [`site_url`][site_url] as set in your configuration is
     prepended to the link.

`alternate.lang`

:   This property must contain an [ISO 639-1 language code] and is used for
    the `hreflang` attribute of the link, improving discoverability via search
    engines.

  [site_url]: https://www.mkdocs.org/user-guide/configuration/#site_url
  [ISO 639-1 language code]: https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes

#### Directionality

While many languages are read `ltr` (left-to-right), Zensical also
supports `rtl` (right-to-left) directionality which is deduced from the
selected language, but can also be set with:

=== "`zensical.toml`"
    ``` toml
    [project.theme]
    direction = "ltr"
    ```

=== "`mkdocs.yml`"
    ``` yaml
    theme:
      direction: ltr
    ```

Click on a tile to change the directionality:

<div class="mdx-switch">
  <button data-md-dir="ltr"><code>ltr</code></button>
  <button data-md-dir="rtl"><code>rtl</code></button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-dir]")
  buttons.forEach(function(button) {
    button.addEventListener("click", function() {
      var attr = this.getAttribute("data-md-dir")
      document.body.dir = attr
      var name = document.querySelector("#__code_2 code span.l")
      name.textContent = attr
    })
  })
</script>

### Customization

#### Custom translations

If you want to customize some of the translations for a language, just follow
the guide on [theme extension] and create a new partial in the `overrides`
folder. Then, import the [translations] of the language as a fallback and only
adjust the ones you want to override:

=== "`overrides/partials/languages/custom.html`"

    ``` html
    <!-- Import translations for language and fallback -->
    {% import "partials/languages/de.html" as language %}
    {% import "partials/languages/en.html" as fallback %} <!-- (1)! -->

    <!-- Define custom translations -->
    {% macro override(key) %}{{ {
      "source.file.date.created": "Erstellt am", <!-- (2)! -->
      "source.file.date.updated": "Aktualisiert am"
    }[key] }}{% endmacro %}

    <!-- Re-export translations -->
    {% macro t(key) %}{{
      override(key) or language.t(key) or fallback.t(key)
    }}{% endmacro %}
    ```

    1.  Note that `en` must always be used as a fallback language, as it's the
        default theme language.

    2.  Check the [list of available languages], pick the translation you want
        to override for your language and add them here.

=== "`zensical.toml`"
    ``` toml
    [project.theme]
    language = "custom"
    ```
=== "`mkdocs.yml`"
    ``` yaml
    theme:
      language: custom
    ```

  [theme extension]: ../customization.md#extending-the-theme
  [translations]: https://github.com/zensical/ui/tree/master/dist/partials/languages

## Logo and icons

When installing Zensical, you immediately get access to _over 10,000 icons_ ready
to be used for customization of specific parts of the theme and when writing
your documentation in Markdown. Not enough? You can also add [additional icons]
with minimal effort.

  [additional icons]: #additional-icons

### Configuration

#### Logo

The logo can be changed to a user-provided image (any type, incl. `*.png` and
`*.svg`) located in the `docs` folder, or to any icon bundled with the theme.
Add the following lines to your configuration to set you own logo from an image
file:

=== "`zensical.toml`"
    ``` toml
    [project.theme]
    logo = "images/logo.png"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      logo: images/logo.png
    ```

To set the logo to use one of the bundled icons, [find a suitable icon] and add
it to the configuration:

=== "`zensical.toml`"
    ``` toml
    [project.theme.icon]
    logo = "lucide/smile"
    ```

=== "`mkdocs.yml`"
    ``` yaml
    theme:
      icon:
        logo: lucide/smile
    ```

[find a suitable icon]: ../authoring/icons-emojis.md#included-icon-sets

Normally, the logo in the header and sidebar links to the homepage of the
documentation, which is the same as `site_url`. This behavior can be changed
with the following configuration:

=== "`zensical.toml`"
    ``` toml
    [project.extra]
    homepage = "https://example.com"
    ```

=== "`mkdocs.yml`"
    ``` yaml
    extra:
      homepage: https://example.com
    ```

#### Favicon

The favicon can be changed to a path pointing to a user-provided image, which
must be located in the `docs` folder. Add the following lines to you
configuration:

=== "`zensical.toml`"
    ``` toml
    [project.theme]
    favicon = "images/favicon.png"
    ```

=== "`mkdocs.yml`"
    ``` yaml
    theme:
      favicon: images/favicon.png
    ```

#### Site icons

Most icons you see on your site, such as navigation icons, can also be changed. For example,
to change the navigation arrows in the footer, add the following lines to your configuration:

=== "`zensical.toml`"
    ``` toml
    [project.theme.icon]
    previous = "fontawesome/solid/angle-left"
    next = "fontawesome/solid/angle-right"
    ```

=== "`mkdocs.yml`"
    ``` yaml
    theme:
      icon:
        previous: fontawesome/solid/angle-left
        next: fontawesome/solid/angle-right
    ```

The following is a complete list of customizable icons used by the theme:

| Icon name    | Purpose                                                                       |
|:-------------|:------------------------------------------------------------------------------|
| `logo`       | See [Logo](#logo)                                                             |
| `menu`       | Open drawer                                                                   |
| `alternate`  | Change language                                                               |
| `search`     | Search icon                                                                   |
| `share`      | Share search                                                                  |
| `close`      | Reset search, dismiss announcements                                           |
| `top`        | Back-to-top button                                                            |
| `edit`       | Edit current page                                                             |
| `view`       | View page source                                                              |
| `repo`       | Repository icon                                                               |
| `admonition` | See [Admonition icons](../authoring/admonitions.md#admonition-icons)          |
| `tag`        | See [Tag icons and identifiers](tags.md#tag-icons-and-identifiers) |
| `previous`   | Previous page in footer, hide search on mobile                                |
| `next`       | Next page in footer                                                           |

### Customization

#### Additional icons

In order to use custom icons, [extend the theme] and create a new folder named
`.icons` in the [`custom_dir`][custom_dir] you want to use for overrides.
Next, add your `*.svg` icons into a subfolder of the `.icons` folder. Let's say
you downloaded and unpacked the [Bootstrap] icon set, and want to add it to
your project documentation. The structure of your project should look like this:

=== "`zensical.toml`"
    ``` { .sh .no-copy }
    .
    ├─ overrides/
    │  └─ .icons/
    │     └─ bootstrap/
    │        └─ *.svg
    └─ zensical.toml
    ```

    Then, add the following lines to `zensical.toml`:

    ``` toml
    [project.theme]
    custom_dir = "overrides"

    [project.markdown_extensions.pymdownx.emoji]
    emoji_index = "zensical.extensions.emoji.twemoji"
    emoji_generator = "zensical.extensions.emoji.to_svg"
    options.custom_icons = ["overrides/.icons"]
    ```

=== "`mkdocs.yml`"
    ``` { .sh .no-copy }
    .
    ├─ overrides/
    │  └─ .icons/
    │     └─ bootstrap/
    │        └─ *.svg
    └─ mkdocs.yml
    ```

    Then, add the following lines to `mkdocs.yml`:

    ``` yaml
    theme:
      custom_dir: overrides

    markdown_extensions:
      - pymdownx.emoji:
          emoji_index: !!python/name:zensical.extensions.emoji.twemoji
          emoji_generator: !!python/name:zensical.extensions.emoji.to_svg
          options:
            custom_icons:
              - overrides/.icons
    ```

You can now use all :fontawesome-brands-bootstrap: Bootstrap icons anywhere in
Markdown files, as well as everywhere icons can be used in your configuration.
However, note that the syntaxes are slightly different:

- __Use icons in configuration__: take the path of the `*.svg` icon file
  starting at the `.icons` folder and drop the file extension, e.g. for
  `.icons/bootstrap/envelope-paper.svg`, use:

    === "`zensical.toml`"
        ``` toml
        [project.theme.icon]
        logo = "bootstrap/envelope-paper"
        ```

    === "`mkdocs.yml`"
        ``` yaml
        theme:
          icon:
            logo: bootstrap/envelope-paper
        ```

- __Use icons in Markdown files__: additionally to taking the path from the
  `.icons` folder as noted above, replace all `/` with `-` and enclose the icon
  shortcode in two colons:

    ```
    :bootstrap-envelope-paper:
    ```

For further notes on icon usage, please consult the [icon reference].

  [extend the theme]: ../customization.md#extending-the-theme
  [custom_dir]: https://www.mkdocs.org/user-guide/configuration/#custom_dir
  [Bootstrap]: https://icons.getbootstrap.com/
  [icon reference]: ../authoring/icons-emojis.md#use-icons

## Data privacy

Zensical offers features to comply with data privacy regulations, as it offers
a native [cookie consent] solution to seek explicit consent from users before
setting up [site analytics].

  [cookie consent]: #cookie-consent
  [site analytics]: analytics.md

### Configuration

#### Cookie consent

Zensical ships a native and extensible cookie consent form which
asks the user for consent prior to sending requests to third parties. Add the
following to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.extra.consent]
    title = "Cookie consent"
    description = """
        We use cookies to recognize your repeated visits and preferences, as well
        as to measure the effectiveness of our documentation and whether users
        find what they're searching for. With your consent, you're helping us to
        make our documentation better.
    """ # (1)!
    ```

    1.  You can add arbitrary HTML tags in the `description`, e.g. to link to your
        terms of service or other parts of the site.

=== "`mkdocs.yml`"

    ``` yaml
    extra:
      consent:
        title: Cookie consent
        description: >- # (1)!
          We use cookies to recognize your repeated visits and preferences, as well
          as to measure the effectiveness of our documentation and whether users
          find what they're searching for. With your consent, you're helping us to
          make our documentation better.
    ```

    1.  You can add arbitrary HTML tags in the `description`, e.g. to link to your
        terms of service or other parts of the site.

The following properties are available:

`consent.title`

:   This required property sets the title of the cookie consent, which is
    rendered at the top of the form and must be set to a non-empty string.

`consent.description`

:   This required property sets the description of the cookie consent, is
    rendered below the title, and may include raw HTML (e.g. a links to the
    terms of service).

`consent.cookies`

:   This property allows to add custom cookies or change the initial `checked`
    state and name of built-in cookies. Currently, the following cookies are
    built-in:

    - __Google Analytics__ – `analytics` (enabled by default)
    - __GitHub__ – `github` (enabled by default)

    Each cookie must receive a unique identifier which is used as a key in the
    `cookies` map, and can be either set to a string, or to a map defining
    `name` and `checked` state:

    Custom cookie name:

    === "`zensical.toml`"

        ``` toml
        [project.extra.consent.cookies]
        analytics = "Custom name"
        ```

    === "`mkdocs.yml`"

        ``` yaml
        extra:
          consent:
            cookies:
              analytics: Custom name
        ```

    Custom initial state:

    === "`zensical.toml`"

        ``` toml
        [project.extra.consent.cookies]
        analytics.name = "Google Analytics"
        checked = false
        ```

    === "`mkdocs.yml`"

        ``` yaml
        extra:
          consent:
            cookies:
              analytics:
                name: Google Analytics
                checked: false
        ```

    Custom cookie:

    === "`zensical.toml`"

        ``` toml
        [project.extra.consent.cookies]
        analytics.name = "Google Analytics" # (1)!
        custom = "Custom cookie"
        ```

        1.  If you define a custom cookie as part of the `cookies` property,
            the `analytics` cookie must be added back explicitly, or analytics
            won't be triggered.

    === "`mkdocs.yml`"

        ``` yaml
        extra:
          consent:
            cookies:
              analytics: Google Analytics # (1)!
              custom: Custom cookie
        ```

        1.  If you define a custom cookie as part of the `cookies` property,
            the `analytics` cookie must be added back explicitly, or analytics
            won't be triggered.

    If Google Analytics was configured, the cookie consent will
    automatically include a setting for the user to disable it. [Custom cookies]
    can be used from JavaScript.

`consent.actions`

:   This property defines which buttons are shown and in which order, e.g., to
    allow the user to accept cookies and manage settings:

    === "`zensical.toml`"
        ``` toml
        [project.extra.consent]
        actions = [
            "accept",
            "manage" # (1)!
        ]
        ```

        1.  If the `manage` settings button is omitted from the `actions` property,
            the settings are always shown.

    === "`mkdocs.yml`"
        ``` yaml
        extra:
          consent:
            actions:
              - accept
              - manage # (1)!
        ```

        1.  If the `manage` settings button is omitted from the `actions` property,
            the settings are always shown.

    The cookie consent form includes three types of buttons:

    - `accept` – Button to accept selected cookies
    - `reject` – Button to reject all cookies
    - `manage` – Button to manage settings

When a user first visits your site, a cookie consent form is rendered:

[![Cookie consent enabled]][Cookie consent enabled]
[![Cookie consent enabled dark]][Cookie consent enabled dark]

  [Cookie consent enabled]: ../assets/screenshots/consent.png#gh-light-mode-only
  [Cookie consent enabled dark]: ../assets/screenshots/consent-dark.png#gh-dark-mode-only

##### Change cookie settings

In order to comply with GDPR, users must be able to change their cookie settings
at any time. This can be done by adding a simple link to your [copyright notice]
in the footer below the copyright message:

=== "`zensical.toml`"
    ``` toml
    [project]
    copyright = """
      Copyright &copy; Zensical LLC –
      <a href="#__consent">Change cookie settings</a>
    """
    ```

=== "`mkdocs.yml`"
    ``` yaml
    copyright: >
      Copyright &copy; Zensical LLC –
      <a href="#__consent">Change cookie settings</a>
    ```

  [copyright notice]: footer.md#copyright-notice

### Customization

#### Custom cookies

If you've customized the [cookie consent] and added a `custom` cookie, the user
will be prompted to accept or reject your custom cookie. Once the user accepts
or rejects the cookie consent, or [changes the settings], the page reloads[^1].
Use [additional JavaScript] to query the result:

  [^1]:
    We reload the page to make interop with custom cookies simpler. If Zensical
    was to implement a callback-based approach, the author would need
    to make sure to correctly update all scripts that use cookies. Additionally,
    the cookie consent is only answered initially, which is why we consider this
    to be a good trade-off of DX and UX.

=== "`docs/javascripts/consent.js`"

    ``` js
    var consent = __md_get("__consent")
    if (consent && consent.custom) {
      /* The user accepted the cookie */
    } else {
      /* The user rejected the cookie */
    }
    ```

=== "`zensical.toml`"
    ``` toml
    [project]
    extra_javascript = ["javascripts/consent.js"]
    ```
=== "`mkdocs.yml`"

    ``` yaml
    extra_javascript:
      - javascripts/consent.js
    ```

  [additional JavaScript]: ../customization.md#additional-javascript
  [changes the settings]: #change-cookie-settings

## Navigation

A clear and concise navigation structure is an important aspect of good project
documentation. Zensical provides several options to configure the behavior of
navigational elements, including [tabs] and [sections], as well as features
such as [instant navigation] and [instant previews].

  [tabs]: #navigation-tabs
  [sections]: #navigation-sections
  [instant navigation]: #instant-navigation
  [instant previews]: #instant-previews

Additional navigation can be configured [in the footer].

[in the footer]: footer.md#navigation

### Configuration

By default, Zensical creates the navigation sidebar on the basis of the folder
structure and content of the Markdown pages. Likewise, it uses a default layout
that can be overridden using various feature flags described on this page.

#### Explicit navigation

If you want to exercise more control over the structure of your navigation, you
can create an explicit definition of the navigation structure in your
configuration file. In the simplest case, you simply list the paths to your
content files, leaving it to Zensical to extract a title for each of them from
the content itself. The paths need to be relative to the [`docs_dir`][docs_dir].

  [docs_dir]: basics.md#docs_dir

=== "`zensical.toml`"

    ``` toml
    [project]
    nav = [
      "index.md",
      "about.md"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    nav:
      - index.md
      - about.md
    ```

Instead of letting Zensical figure out the title to use for the navigation entry
for a page, you can also explicitly specify a title:

=== "`zensical.toml`"

    ``` toml
    [project]
    nav = [
      {"Home" = "index.md"},
      {"About" = "about.md"}
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    nav:
      - Home: index.md
      - About: about.md
    ```

##### Navigation sections

You can define navigation sections to create a navigation hierarchy that guides
your users to the information they require.

=== "`zensical.toml`"

      ``` toml
      [project]
      nav = [
        {"Home" = "index.md"},
        {"About" = [
           "about/index.md",
           "about/vision.md",
           "about/team.md"
        ]}
      ]
      ```

=== "`mkdocs.yml`"

      ``` yaml
      nav:
        - Home: index.md
        - About:
          - about/index.md
          - about/vision.md
          - about/team.md
      ```

##### External links

Navigation items typically provide a path to a Markdown page. However, any
string that cannot be resolved to a Markdown page is treated as a URL.

=== "`zensical.toml`"

      ``` toml
      [project]
      nav = [
        {"GitHub Repo" = "https://github.com/zensical/docs"}
      ]
      ```

=== "`mkdocs.yml`"

      ``` yaml
      nav:
        - GitHub Repo: https://github.com/zensical/docs
      ```

The "GitHub Repo" navigation entry takes the user to the repository for the
Zensical Documentation.

#### Instant navigation

When instant navigation is enabled, clicks on all internal links will be
intercepted and dispatched via [XHR] without fully reloading the page. Add
the following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.instant"
    ]
    ```
=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.instant
    ```

The resulting page is parsed and injected and all event handlers and components
are rebound automatically, i.e., __Zensical now behaves like a Single
Page Application__. Also, the search index is persisted through navigation,
which is especially useful for large documentation sites.

!!! info "The [`site_url`][site_url] setting must be set"

    Note that you must set [`site_url`][site_url] when using instant
    navigation, as instant navigation relies on the generated `sitemap.xml`
    which will be empty if this setting is omitted.

  [XHR]: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest

##### Instant prefetching

Instant prefetching is a new experimental feature that will start to fetch a
page once the user hovers over a link. This reduces the perceived loading time
for the user, especially on slow connections, as the page will be available
immediately upon navigation. Enable it with:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.instant",
        "navigation.instant.prefetch"
    ]
    ```
=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.instant
        - navigation.instant.prefetch
    ```

##### Progress indicator

In order to provide a better user experience on slow connections when using
instant navigation, a progress indicator can be enabled. It will be shown at
the top of the page and will be hidden once the page has fully loaded. You can
enable it in your configuration with:

=== "`zensical.toml`"
    ``` toml
    [project.theme]
    features = [
        "navigation.instant",
        "navigation.instant.progress"
    ]
    ```

=== "`mkdocs.yml`"
    ``` yaml
    theme:
      features:
        - navigation.instant
        - navigation.instant.progress
    ```

The progress indicator will only show if the page hasn't finished loading after
400ms, so that fast connections will never show it for a better instant
experience.

#### Instant previews

Instant previews allow the user to preview another site of your documentation
without navigating to it. They can be very helpful to keep the user in context.
Instant previews can be enabled on any header link with the `data-preview`
attribute:

```` markdown title="Link with instant preview"
``` markdown
[Attribute Lists](#){ data-preview }
```
````

<div class="result" markdown>

[Attribute Lists](extensions/python-markdown.md#attribute-lists){ data-preview }

</div>

!!! info "Limitations"

    Instant previews are still an experimental feature and currently limited to
    headerlinks. This means, you can use them on any internal link that points
    to a header on another page, but not other elements with `id` attributes.

##### Automatic previews

The recommended way to work with instant previews is to use the Markdown
extension that is included with Zensical, as it allows you to enable
instant previews on a per-page or per-section level for your documentation:

=== "`zensical.toml`"

    ``` toml
    [project.markdown_extensions.zensical.extensions.preview]
    configurations = [
        { targets.include = [
            "customization.md",
            "setup/extensions/*"
        ]}
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    markdown_extensions:
      - material.extensions.preview:
          configurations:
            - targets:
                include:
                  - customization.md
                  - setup/extensions/*
    ```

The above configuration is what we use for our documentation. We've enabled
instant previews for our changelogs, customization guide and for all Markdown
extensions in the setup guide.

??? example "Full configuration example"

    === "`zensical.toml`"
        ``` toml
        [[project.markdown_extensions.zensical.extensions.preview.configurations]]
        sources.include = [...]
        sources.exclude = [...]
        targets.include = [...]
        targets.exclude = [...]
        ```

    === "`mkdocs.yml`"
        ``` yaml
        markdown_extensions:
          - material.extensions.preview:
              configurations:
                - sources: # (1)!
                    include:
                      - ...
                    exclude:
                      - ...
                  targets: # (2)!
                    include:
                      - ...
                    exclude:
                      - ...
        ```

        1.  Sources specify the pages _on_ which instant previews should be enabled.
            If this setting is omitted, instant previews will be enabled on all
            pages. You can use patterns to include or exclude pages. Exclusion is
            evaluated on top of inclusion, so if a page is matched by both, it will
            be excluded.

            Note that you can define multiple items under the `configurations`
            setting, which allows to precisely control where instant previews are
            shown.

        2.  Targets specify the pages _to_ which instant previews should be enabled.
            This is the recommended way to enable instant previews.
---

!!! info "The [`site_url`][site_url] setting must be set"

    Note that you must set [`site_url`][site_url] when using instant
    previews, as instant previews rely on the generated `sitemap.xml`
    which will be empty if this setting is omitted.

#### Anchor tracking

When anchor tracking is enabled, the URL in the address bar is automatically
updated with the active anchor as highlighted in the table of contents. Add the
following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.tracking"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.tracking
    ```

#### Navigation tabs

When tabs are enabled, top-level sections are rendered in a menu layer below
the header for viewports above `1220px`, but remain as-is on mobile. Add the
following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.tabs"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.tabs
    ```

===! "With tabs"

    [![Navigation tabs enabled]][Navigation tabs enabled]
    [![Navigation tabs enabled dark]][Navigation tabs enabled dark]

=== "Without"

    [![Navigation tabs disabled]][Navigation tabs disabled]
    [![Navigation tabs disabled dark]][Navigation tabs disabled dark]

##### Sticky navigation tabs

When sticky tabs are enabled, navigation tabs will lock below the header and
always remain visible when scrolling down. Just add the following two feature
flags to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.tabs",
        "navigation.tabs.sticky"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.tabs
        - navigation.tabs.sticky
    ```

#### Navigation sections

When sections are enabled, top-level sections are rendered as groups in the
sidebar for viewports above `1220px`, but remain as-is on mobile. Add the
following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.sections"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.sections
    ```

===! "With sections"

    [![Navigation sections enabled]][Navigation sections enabled]
    [![Navigation sections enabled dark]][Navigation sections enabled dark]

=== "Without"

    [![Navigation sections disabled]][Navigation sections disabled]
    [![Navigation sections disabled dark]][Navigation sections disabled dark]

Both feature flags, [`navigation.tabs`][tabs] and
[`navigation.sections`][sections], can be combined with each other. If both
feature flags are enabled, sections are rendered for level 2 navigation items.

#### Navigation expansion

When expansion is enabled, the left sidebar will expand all collapsible
subsections by default, so the user doesn't have to open subsections manually.
Add the following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.expand"
    ]
    ```
=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.expand
    ```

===! "With expansion"

    [![Navigation expansion enabled]][Navigation expansion enabled]
    [![Navigation expansion enabled dark]][Navigation expansion enabled dark]

=== "Without"

    [![Navigation expansion disabled]][Navigation expansion disabled]
    [![Navigation expansion disabled dark]][Navigation expansion disabled dark]

#### Navigation path <small>Breadcrumbs</small> { #navigation-path data-toc-label="Navigation path" }

When navigation paths are activated, a breadcrumb navigation is rendered above
the title of each page, which might make orientation easier for users visiting your
documentation on devices with smaller screens. Add the following lines to
your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.path"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.path
    ```

===! "With breadcrumbs"

    [![Navigation path enabled]][Navigation path enabled]
    [![Navigation path enabled dark]][Navigation path enabled dark]

=== "Without"

    [![Navigation path disabled]][Navigation path disabled]
    [![Navigation path disabled dark]][Navigation path disabled dark]

#### Navigation pruning

When pruning is enabled, only the visible navigation items are included in the
rendered HTML, __reducing the size of the built site by 33% or more__. Add the
following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.prune" # (1)!
    ]
    ```

    1.  This feature flag is not compatible with
        [`navigation.expand`][navigation.expand], as navigation expansion requires
        the complete navigation structure.

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.prune # (1)!
    ```

    1.  This feature flag is not compatible with
        [`navigation.expand`][navigation.expand], as navigation expansion requires
        the complete navigation structure.

This feature flag is especially useful for documentation sites with thousands of
pages, as the navigation makes up a significant fraction of the HTML. Navigation
pruning will replace all expandable sections with links to the first page in
that section (or the section index page).

#### Section index pages

When section index pages are enabled, documents can be directly attached to
sections, which is particularly useful for providing overview pages. Add the
following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.indexes" # (1)!
    ]
    ```

    1.  This feature flag is not compatible with [`toc.integrate`][toc.integrate],
        as sections cannot host the table of contents due to missing space.

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.indexes # (1)!
    ```

    1.  This feature flag is not compatible with [`toc.integrate`][toc.integrate],
        as sections cannot host the table of contents due to missing space.

In order to link a page to a section, create a new document with the name
`index.md` in the respective folder, and add it to the beginning of your
navigation section:

=== "`zensical.toml`"

    ``` toml
    [project]
    nav = [
        {"Section" = [
            "section/index.md", # (1)!
            {"Page 1" = "section/page-1.md"},
            ...
            {"Page n" = "section/page-n.md"}

        ]}
    ]
    ```

    1.  `README.md` is also considered an index page.

=== "`mkdocs.yml`"

    ``` yaml
    nav:
      - Section:
        - section/index.md # (1)!
        - Page 1: section/page-1.md
        ...
        - Page n: section/page-n.md
    ```

    1.  `README.md` is also considered an index page.

#### Table of contents

##### Anchor following

When anchor following for the [table of contents] is enabled, the sidebar is
automatically scrolled so that the active anchor is always visible. Add the
following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "toc.follow"
    ]
    ```
=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - toc.follow
    ```

##### Navigation integration

When navigation integration for the [table of contents] is enabled, it is always
rendered as part of the navigation sidebar on the left. Add the following lines
to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "toc.integrate" # (1)!
    ]
    ```

    1.  This feature flag is not compatible with
        [`navigation.indexes`][navigation.indexes], as sections cannot host the
        table of contents due to missing space.

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - toc.integrate # (1)!
    ```

    1.  This feature flag is not compatible with
        [`navigation.indexes`][navigation.indexes], as sections cannot host the
        table of contents due to missing space.

===! "With navigation integration"

    [![Navigation integration enabled]][Navigation integration enabled]
    [![Navigation integration enabled dark]][Navigation integration enabled dark]

=== "Without"

    [![Navigation integration disabled]][Navigation integration disabled]
    [![Navigation integration disabled dark]][Navigation integration disabled dark]

  [table of contents]: extensions/python-markdown.md#table-of-contents

#### Back-to-top button

A back-to-top button can be shown when the user, after scrolling down, starts
to scroll up again. It's rendered centered and just below the header. Add the
following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.top"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.top
    ```

### Usage

#### Hide the sidebars

The navigation and/or table of contents sidebars can be hidden for a document
with the front matter `hide` property. Add the following lines at the top of a
Markdown file:

``` yaml
---
hide:
  - navigation
  - toc
---

## Page title
...
```

#### Hide the navigation path

While the [navigation path] is rendered above the main headline, sometimes, it
might be desirable to hide it for a specific page, which can be achieved with
the front matter `hide` property:

``` yaml
---
hide:
  - path
---

## Page title
...
```

  [navigation path]: #navigation-path

### Customization

#### Content area width

The width of the content area is set so the length of each line doesn't exceed
80-100 characters, depending on the width of the characters. While this
is a reasonable default, as longer lines tend to be harder to read, it may be
desirable to increase the overall width of the content area, or even make it
stretch to the entire available space.

This can easily be achieved with an [additional style sheet] and a few lines
of CSS:

=== "`docs/stylesheets/extra.css`"

    ``` css
    .md-grid {
      max-width: 1440px; /* (1)! */
    }
    ```

    1.  If you want the content area to always stretch to the available screen
        space, reset `max-width` with the following CSS:

        ``` css
        .md-grid {
          max-width: initial;
        }
        ```

=== "`zensical.toml`"

    ``` toml
    [project]
    extra_css = ["stylesheets/extra.css"]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra_css:
      - stylesheets/extra.css
    ```

## Search

Zensical offers seamless client-side search functionality, eliminating the need
to integrate third-party services that may not comply with privacy regulations.
Additionally, the search works [offline], enabling you to distribute
documentation as a download.

!!! info "We're interested in your feedback"

    Zensical ships a completely new search engine that we've written from
    scratch and we're eager to hear your feedback! We're continuously working
    on it, and will release it as a [standalone Open Source project] in 2026.

!!! warning "Search is currently only available in English"

    At the moment, the search interface is not localized, since it's an entirely
    new implementation. Thus, it does not make sense for us to carry over the
    search localization from Material for MkDocs, as its tightly coupled to
    the old search implementation, and we're still changing too much.

    This does not impact multi-lingual search.

  [offline]: offline.md

### Configuration

The built-in search module is seamlessly integrated with Zensical,
adding multilingual client-side search, and is enabled by default, so there's
not need for additional configuration.

#### Search highlighting

When search highlighting is enabled and a user clicks on a search result,
Zensical will highlight all occurrences after following the link.
Add the following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "search.highlight"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - search.highlight
    ```

### Usage

#### Search exclusion

##### Exclude a page

Pages can be excluded from search with the front matter `search.exclude`
property, removing them from the index. Add the following lines at the top of a
Markdown file:

``` yaml
---
search:
  exclude: true
---

## Page title
...
```

##### Exclude a section

When [Attribute Lists] is enabled, specific sections of pages can be excluded
from search by adding the `data-search-exclude` pragma after a Markdown
heading:

``` markdown
## Page title

### Section 1

The content of this section is included

### Section 2 { data-search-exclude }

The content of this section is excluded
```

  [Attribute Lists]: extensions/python-markdown.md#attribute-lists

##### Exclude a block

When [Attribute Lists] is enabled, specific sections of pages can be excluded
from search by adding the `data-search-exclude` pragma after a Markdown
inline- or block-level element:

``` markdown
## Page title

The content of this block is included

The content of this block is excluded
{ data-search-exclude }
```

## Social cards

We are currently working towards [feature parity] with Material for MkDocs to
provide even more powerful social cards functionality for Zensical soon.

!!! tip "Want to stay in the loop?"

    Join our [newsletter] to receive a monthly update on our progress, about
    new features in our pipeline, and what we have planned next. We'll also
    share exclusive behind-the-scenes insights and invites to workshops.

  [feature parity]: https://zensical.org/compatibility/features/

## Tags

Zensical adds first-class support for categorizing pages with tags, which allows
users to discover related pages via the [search]. If your documentation is
large, tags can help to discover relevant information faster.

  [search]: search.md

### Configuration

The built-in tags functionality lets you categorize any page with tags
as part of the [metadata] of the page. Tags are supported by default, no
configuration needed.

!!! info "Tag listings are currently not supported"
    As we are working towards [feature parity] with Material for MkDocs, we
    will be adding features such as tag indexes that are implemented as part of
    the tags plugin in Material for MkDocs.

  [metadata]: ../authoring/frontmatter.md

#### Tag icons and identifiers

Each tag can be associated with an icon, which is rendered inside the tag.
Before assigning icons to tags, associate each tag with a unique identifier,
by adding the following to your configuration:

=== "`zensical.toml`"

    ```toml
    [project.extra.tags]
    <tag> = "<identifier>"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra:
      tags:
        <tag>: <identifier>
    ```

The identifier can only include alphanumeric characters, as well as dashes
and underscores. For example, if you have a tag `Compatibility`, you can
set `compat` as an identifier:

=== "`zensical.toml`"

    ``` toml
    [project.extra.tags]
    Compatibility = "compat"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra:
      tags:
        Compatibility: compat
    ```

Identifiers can be reused between tags to assign groups of tags the same icon.
Tags that are not explicitly associated with an identifier will use the default
tag icon.

Next, each identifier can be associated with an icon, even a [custom icon], by
adding the following lines to you configuration under the `theme.icon` configuration
setting:

=== "`zensical.toml`"

    ```toml
    [project.theme.icon.tag]
    default = "<icon>"
    <identifier> = "<icon>"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      icon:
        tag:
          default: <icon>
          <identifier>: <icon>
    ```

??? example "Expand to see an example"

    === "`zensical.toml`"
        ```toml
        [project.theme.icon.tag]
        default = "lucide/hash"
        html = "fontawesome/brands/html5"
        js = "fontawesome/brands/js"
        css = "fontawesome/brands/css3"

        [project.extra.tags]
        HTML5 = "html"
        JavaScript = "js"
        CSS = "cs"
        ```

    === "`mkdocs.yml`"
        ``` yaml
        theme:
          icon:
            tag:
              default: lucide/hash
              html: fontawesome/brands/html5
              js: fontawesome/brands/js
              css:  fontawesome/brands/css3
        extra:
          tags:
            HTML5: html
            JavaScript: js
            CSS: css
        ```

  [custom icon]: logo-and-icons.md#additional-icons

### Usage

#### Add tags

Tags can be added for a document with the front matter `tags` property. They can
also be used in search without any further configuration. Add the following
lines at the top of a Markdown file:

``` yaml
---
tags:
  - HTML5
  - JavaScript
  - CSS
---

## Page title
...
```

The page will now render with those tags at the bottom of the page and your
users will be able to filter by these tags in the search.

#### Hide tags on a page

While the tags are rendered at the bottom of each page, sometimes, it might be
desirable to hide them for a specific page, which can be achieved with the
front matter `hide` property:

``` yaml
---
hide:
  - tags
---

## Page title
...
```

## Header

The header can be customized to show an announcement bar that disappears upon scrolling, and provides some options for further configuration. It also includes
the [search bar] and a place to display your project's [git repository], as
explained in those dedicated guides.

  [search bar]: search.md
  [git repository]: repository.md

### Configuration

#### Automatic hiding

When autohiding is enabled, the header is automatically hidden when the
user scrolls past a certain threshold, leaving more space for content. Add the
following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "header.autohide"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - header.autohide
    ```

#### Announcement bar

Zensical includes an announcement bar, which is the perfect place to
display project news or other important information to the user. When the user
scrolls past the header, the bar will automatically disappear. In order to add
an announcement bar, [extend the theme] and [override the `announce`
block][overriding blocks], which is empty by default:

``` html
{% extends "base.html" %}

{% block announce %}
  <!-- Add announcement here, including arbitrary HTML -->
{% endblock %}
```

  [overriding blocks]: ../customization.md#overriding-blocks

##### Mark as read

For temporary announcements that can be marked as read by the user, a button to
dismiss the current announcement can be included. Add the following lines to
your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "announce.dismiss"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - announce.dismiss
    ```

When the user clicks the button, the current announcement is dismissed and not
displayed again until the content of the announcement changes.

## Footer

The footer of your documentation hosts the copyright notice, links to the
previous and next page, as well as links to your social media profiles, all of
which can be enable via configuration.

### Configuration

#### Navigation

The footer can include links to the previous and next page of the current page.
If you wish to enable this behavior, add the following lines to your
configuration:

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
        "navigation.footer"
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - navigation.footer
    ```

#### Social links

Social links are rendered next to the copyright notice as part of the
footer of your project documentation. Add a list of social links in your
configuration with:

=== "`zensical.toml`"

    ``` toml
    [[project.extra.social]]
    icon = "fontawesome/brands/x-twitter"
    link = "https://x.com/zensical"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra:
      social:
        - icon: fontawesome/brands/x-twitter
          link: https://x.com/zensical
    ```

The following properties are available for each link:

`social.icon`
:   This property must contain a valid path to any [icon bundled with the theme],
    or the build will not succeed. Some popular choices:

    * :fontawesome-brands-github: – `fontawesome/brands/github`
    * :fontawesome-brands-gitlab: – `fontawesome/brands/gitlab`
    * :fontawesome-brands-x-twitter: – `fontawesome/brands/x-twitter`
    * :fontawesome-brands-mastodon: – `fontawesome/brands/mastodon`
      <small>automatically adds [`rel=me`][rel=me]</small>
    * :fontawesome-brands-docker: – `fontawesome/brands/docker`
    * :fontawesome-brands-facebook: – `fontawesome/brands/facebook`
    * :fontawesome-brands-instagram: – `fontawesome/brands/instagram`
    * :fontawesome-brands-linkedin: – `fontawesome/brands/linkedin`
    * :fontawesome-brands-slack: – `fontawesome/brands/slack`
    * :fontawesome-brands-discord: – `fontawesome/brands/discord`

[icon bundled with the theme]: ../authoring/icons-emojis.md

`social.link`

:   This property must be set to a relative or absolute URL including the URI
    scheme. All URI schemes are supported, including `mailto` and `bitcoin`.
    An example for adding a Mastodon link was given above. To add an Email link,
    add this:

    === "`zensical.toml`"
        ``` toml
        [[project.extra.social]]
        icon = "fontawesome/solid/paper-plane"
        link = "mailto:<email-address>"
        ```

    === "`mkdocs.yml`"
        ``` yaml
        extra:
          social:
            - icon: fontawesome/solid/paper-plane
              link: mailto:<email-address>
        ```

`social.name`

:   This property is used as the link's `title` attribute and can be set to a
    discernable name to improve accessibility. The default title is the domain
    name from the link, if available.

    === "`zensical.toml`"
        ``` toml
        [[project.extra.social]]
        icon = "fontawesome/brands/x"
        link = "https://x.com/zensical"
        name = "Zensical on X"
        ```

    === "`mkdocs.yml`"
        ``` yaml
        extra:
          social:
            - icon: fontawesome/brands/mastodon
              link: https://fosstodon.org/@squidfunk
              name: Zensical on Mastodon
        ```

#### Copyright notice

A custom copyright banner can be rendered as part of the footer, which is
displayed next to the social links. It can be defined as part of your
configuration file:

=== "`zensical.toml`"

    ``` toml
    [project]
    copyright = "Copyright &copy; <year> <name>"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    copyright: "Copyright &copy; <year> <name>"
    ```

#### Generator notice

The footer displays a _Made with Zensical_ notice to denote how the site was
generated. The notice can be removed with the following option:

=== "`zensical.toml`"

    ``` toml
    [project.extra]
    generator = false
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra:
      generator: false
    ```

!!! info "Please read this before removing the generator notice"

    The subtle __Made with Zensical__ hint in the footer is a great way to
    spread the word about Zensical. We offer Zensical as Free and Open Source
    software under the permissive MIT license. This is one of the ways you can
    help ensure that the Zensical community grows and thrives, ultimately
    helping to make it a sustainable project.

### Usage

#### Hiding prev/next links

The footer navigation showing links to the previous and next page can be hidden
with the front matter `hide` property. Use this when the content of the page
that is adjacent to the current page is not really related. Add the following
lines at the top of a Markdown file:

``` yaml
---
hide:
  - footer
---

## Page title
...
```

## Repository

If your documentation is related to source code, Zensical provides the ability
to display information about the project's repository as part of the static site,
including stars and forks.

### Configuration

In order to display a link to the repository of your project as part of your
documentation, set `repo_url` in your configuration to the public URL of your
repository, e.g.:

=== "`zensical.toml`"
    ``` toml
    [project]
    repo_url = "https://github.com/zensical/zensical"
    ```

=== "`mkdocs.yml`"
    ``` yaml
    repo_url: https://github.com/zensical/zensical
    ```

The link to the repository will be rendered next to the search bar on big
screens. Additionally, for public repositories hosted on [GitHub] or [GitLab],
the latest release tag[^1], as well as the number of stars and forks, are
automatically requested and rendered.

  [^1]:
    Unfortunately, GitHub only provides an API endpoint to obtain the [latest
    release] - not the latest tag. Thus, make sure to [create a release] (not
    pre-release) for the latest tag you want to display next to the number of
    stars and forks. For GitLab, although it is possible to get a [list of tags
    sorted by update time], the [equivalent API endpoint] is used. So, make sure
    you also [create a release for GitLab repositories].

  [latest release]: https://docs.github.com/en/rest/releases/releases#get-the-latest-release
  [create a release]: https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository#creating-a-release
  [list of tags sorted by update time]: https://docs.gitlab.com/ee/api/tags.html#list-project-repository-tags
  [equivalent API endpoint]: https://docs.gitlab.com/ee/api/releases/#get-the-latest-release
  [create a release for GitLab repositories]: https://docs.gitlab.com/ee/user/project/releases/#create-a-release

#### Repository name

Zensical will infer the source provider by examining the URL and try to set the
_repository name_ automatically. If you wish to customize the name, set
`repo_name` in your configuration:

=== "`zensical.toml`"

    ``` toml
    [project]
    repo_name = "zensical/zensical"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    repo_name: zensical/zensical
    ```

#### Repository icon

While the default repository icon is a generic git icon, it can be set to
any icon bundled with the theme by referencing a valid icon path in
`mkdocs.yml`:

``` yaml
theme:
  icon:
    repo: fontawesome/brands/git-alt
```

You can use icons from any of the [available icon sets] or use one of these
popular choices:

- :fontawesome-brands-git: – `fontawesome/brands/git`
- :fontawesome-brands-git-alt: – `fontawesome/brands/git-alt`
- :fontawesome-brands-github: – `fontawesome/brands/github`
- :fontawesome-brands-github-alt: – `fontawesome/brands/github-alt`
- :fontawesome-brands-gitlab: – `fontawesome/brands/gitlab`
- :fontawesome-brands-gitkraken: – `fontawesome/brands/gitkraken`
- :fontawesome-brands-bitbucket: – `fontawesome/brands/bitbucket`

#### Code actions

If the [repository URL] points to a valid [GitHub], [GitLab] or [Bitbucket]
repository, Zensical provides a setting called `edit_uri`, which
resolves to the subfolder where your documentation is hosted.

If your default branch is called `main`, change the setting to:

=== "`zensical.toml`"

    ``` toml
    [project]
    edit_uri = "edit/main/docs/"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    edit_uri: edit/main/docs/
    ```

After making sure that `edit_uri` is correctly configured, buttons for code
actions can be added. Two types of code actions are supported: `edit` and `view`
(GitHub only):

=== "`zensical.toml`"

    ``` toml
    [project.theme]
    features = [
      "content.action.edit", # Edit this page
      "content.action.view"  # View source of this page
    ]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      features:
        - content.action.edit # Edit this page
        - content.action.view  # View source of this page
    ```

The icon of the edit and view buttons can be changed with the following lines:

=== "`zensical.toml`"

    ``` toml
    [project.theme.icon]
    edit = "material/pencil"
    view = "material/eye"
    ```

=== "`mkdocs.yml`"

    ``` yaml
    theme:
      icon:
        edit: material/pencil
        view: material/eye
    ```

  [repository URL]: #repository
  [GitHub]: https://github.com/
  [GitLab]: https://about.gitlab.com/
  [Bitbucket]: https://bitbucket.org/

## Comment system

Zensical allows to easily add the third-party comment system of your choice to
the footer of any page by using [theme extension]. As an example, we'll be
integrating [Giscus], which is Open Source, free, and uses GitHub discussions
as a backend.

  [Giscus]: https://giscus.app/

### Customization

#### Giscus integration

Before you can use [Giscus], you need to complete the following steps:

1. __Install the [Giscus GitHub App]__ and grant access to the repository
    that should host comments as GitHub discussions. Note that this can be a
    repository different from your documentation.
2. __Visit [Giscus] and generate the snippet__ through their configuration tool
    to load the comment system. Copy the snippet for the next step. The
    resulting snippet should look similar to this:

    ``` html
    <script
      src="https://giscus.app/client.js"
      data-repo="<username>/<repository>"
      data-repo-id="..."
      data-category="..."
      data-category-id="..."
      data-mapping="pathname"
      data-reactions-enabled="1"
      data-emit-metadata="1"
      data-theme="light"
      data-lang="en"
      crossorigin="anonymous"
      async
    >
    </script>
    ```

The [`comments.html`][comments] partial (empty by default) is the best place to
add the snippet generated by [Giscus]. Follow the guide on [theme extension]
and [override the `comments.html` partial][overriding partials] with:

``` html hl_lines="3"
{% if page.meta.comments %}
  <h2 id="__comments">{{ lang.t("meta.comments") }}</h2>
  <!-- Insert generated snippet here -->

  <!-- Synchronize Giscus theme with palette -->
  <script>
    var giscus = document.querySelector("script[src*=giscus]")

    // Set palette on initial load
    var palette = __md_get("__palette")
    if (palette && typeof palette.color === "object") {
      var theme = palette.color.scheme === "slate"
        ? "transparent_dark"
        : "light"

      // Instruct Giscus to set theme
      giscus.setAttribute("data-theme", theme) // (1)!
    }

    // Register event handlers after documented loaded
    document.addEventListener("DOMContentLoaded", function() {
      var ref = document.querySelector("[data-md-component=palette]")
      ref.addEventListener("change", function() {
        var palette = __md_get("__palette")
        if (palette && typeof palette.color === "object") {
          var theme = palette.color.scheme === "slate"
            ? "transparent_dark"
            : "light"

          // Instruct Giscus to change theme
          var frame = document.querySelector(".giscus-frame")
          frame.contentWindow.postMessage(
            { giscus: { setConfig: { theme } } },
            "https://giscus.app"
          )
        }
      })
    })
  </script>
{% endif %}
```

1. This code block ensures that [Giscus] renders with a dark theme when the
    palette is set to `slate`. Note that multiple dark themes are available,
    so you can change it to your liking.

Replace the highlighted line with the snippet you generated with the [Giscus]
configuration tool in the previous step. If you copied the snippet from above,
you can enable comments on a page by setting the `comments` front matter
property to `true`:

``` yaml
---
comments: true
---

## Page title
...
```

  [Giscus GitHub App]: https://github.com/apps/giscus
  [comments]: https://github.com/zensical/ui/blob/master/dist/partials/comments.html
  [overriding partials]: ../customization.md#overriding-partials

## Offline usage

In order to build documentation and either offer it as a download or ship it
with your product, Zensical can ensure that [site search] works even when
accessed from the file system.

### Configuration

The offline functionality makes sure that the [site search] works when you
distribute the contents of your [site directory] as a download. Simply add
the following lines to your configuration:

=== "`zensical.toml`"

    ``` toml
    [project.plugins.offline]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    plugins:
      - offline
    ```

!!! info "Notes on our upcoming module system"

    We're working on a more comprehensive module system to manage functionality
    like this in a more flexible way. The fact that the scope is called
    `plugins` is a carry-over from Material for MkDocs and will be automatically
    replaced in a future version.

  [site search]: search.md
  [site directory]: basics.md#site_dir

The following settings are available:

`config.enabled`
: Use this setting to enable or disable the plugin when building your
  project.

=== "`zensical.toml`"

    ``` toml
    [project.plugins.offline]
    enabled = false
    ```

    There is no way to turn the offline functionality off or on
    based on an environment variable as in a `mkdocs.yml`. We are
    working towards [feature parity] and will be providing a more
    comprehensive system for managing [variants] in due course.

=== "`mkdocs.yml`"

    ``` yaml
    plugins:
    - offline:
        enabled: !ENV [OFFLINE, false]
    ```

##### Limitations

Zensical offers many interactive features, some of which will not
work from the file system due to the restrictions of modern browsers: all
features that use the `fetch` API will error.

Thus, when building for offline usage, make sure to disable the following
configuration settings: [instant navigation], [site analytics], [git repository],
and [comment systems].

  [Comment systems]: comment-system.md
