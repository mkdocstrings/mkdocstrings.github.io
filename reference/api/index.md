# mkdocstrings

mkdocstrings package.

Automatic documentation from sources, for MkDocs.

Modules:

- **`extension`** – Deprecated. Import from mkdocstrings directly.
- **`handlers`** – Deprecated. Import from mkdocstrings directly.
- **`inventory`** – Deprecated. Import from mkdocstrings directly.
- **`loggers`** – Deprecated. Import from mkdocstrings directly.
- **`plugin`** – Deprecated. Import from mkdocstrings directly.

Classes:

- **`AutoDocProcessor`** – Our "autodoc" Markdown block processor.
- **`BaseHandler`** – The base handler class.
- **`CollectionError`** – An exception raised when some collection of data failed.
- **`Handlers`** – A collection of handlers.
- **`HeadingShiftingTreeprocessor`** – Shift levels of all Markdown headings according to the configured base level.
- **`Highlighter`** – Code highlighter that tries to match the Markdown configuration.
- **`IdPrependingTreeprocessor`** – Prepend the configured prefix to IDs of all HTML elements.
- **`Inventory`** – Inventory of collected and rendered objects.
- **`InventoryItem`** – Inventory item.
- **`LoggerAdapter`** – A logger adapter to prefix messages.
- **`MkdocstringsExtension`** – Our Markdown extension.
- **`MkdocstringsInnerExtension`** – Extension that should always be added to Markdown sub-documents that handlers request (and only them).
- **`MkdocstringsPlugin`** – An mkdocs plugin.
- **`ParagraphStrippingTreeprocessor`** – Unwraps the <p> element around the whole output.
- **`PluginConfig`** – The configuration options of mkdocstrings, written in mkdocs.yml.
- **`TemplateLogger`** – A wrapper class to allow logging in templates.
- **`ThemeNotSupported`** – An exception raised to tell a theme is not supported.

Functions:

- **`do_any`** – Check if at least one of the item in the sequence evaluates to true.
- **`get_logger`** – Return a pre-configured logger.
- **`get_template_logger`** – Return a logger usable in templates.
- **`get_template_logger_function`** – Create a wrapper function that automatically receives the Jinja template context.
- **`get_template_path`** – Return the path to the template currently using the given context.

Attributes:

- **`CollectorItem`** – The type of the item returned by the collect method of a handler.
- **`HandlerConfig`** – The type of the configuration of a handler.
- **`HandlerOptions`** – The type of the options passed to a handler.
- **`TEMPLATES_DIRS`** (`Sequence[Path]`) – The directories where the handler templates are located.

## CollectorItem

```python
CollectorItem = Any
```

The type of the item returned by the `collect` method of a handler.

## HandlerConfig

```python
HandlerConfig = Any
```

The type of the configuration of a handler.

## HandlerOptions

```python
HandlerOptions = Any
```

The type of the options passed to a handler.

## TEMPLATES_DIRS

```python
TEMPLATES_DIRS: Sequence[Path] = tuple(__path__)
```

The directories where the handler templates are located.

## AutoDocProcessor

```python
AutoDocProcessor(
    md: Markdown,
    *,
    handlers: Handlers,
    autorefs: AutorefsPlugin,
)
```

Bases: `BlockProcessor`

Our "autodoc" Markdown block processor.

It has a test method that tells if a block matches a criterion, and a run method that processes it.

It also has utility methods allowing to get handlers and their configuration easily, useful when processing a matched block.

Parameters:

- ### **`md`**

  (`Markdown`) – A markdown.Markdown instance.

- ### **`handlers`**

  (`Handlers`) – The handlers container.

- ### **`autorefs`**

  (`AutorefsPlugin`) – The autorefs plugin instance.

Methods:

- **`run`** – Run code on the matched blocks.
- **`test`** – Match our autodoc instructions.

Attributes:

- **`md`** – The Markdown instance.
- **`regex`** – The regular expression to match our autodoc instructions.

### md

```python
md = md
```

The Markdown instance.

### regex

```python
regex = compile(
    "^(?P<heading>#{1,6} *|)::: ?(?P<name>.+?) *$",
    flags=MULTILINE,
)
```

The regular expression to match our autodoc instructions.

### run

```python
run(parent: Element, blocks: MutableSequence[str]) -> None
```

Run code on the matched blocks.

The identifier and configuration lines are retrieved from a matched block and used to collect and render an object.

Parameters:

- #### **`parent`**

  (`Element`) – The parent element in the XML tree.

- #### **`blocks`**

  (`MutableSequence[str]`) – The rest of the blocks to be processed.

### test

```python
test(parent: Element, block: str) -> bool
```

Match our autodoc instructions.

Parameters:

- #### **`parent`**

  (`Element`) – The parent element in the XML tree.

- #### **`block`**

  (`str`) – The block to be tested.

Returns:

- `bool` – Whether this block should be processed or not.

## BaseHandler

```python
BaseHandler(*args: Any, **kwargs: Any)
```

The base handler class.

Inherit from this class to implement a handler.

You will have to implement the `collect` and `render` methods. You can also implement the `teardown` method, and override the `update_env` method, to add more filters to the Jinja environment, making them available in your Jinja templates.

To define a fallback theme, add a `fallback_theme` class-variable. To add custom CSS, add an `extra_css` variable or create an 'style.css' file beside the templates.

If the given theme is not supported (it does not exist), it will look for a `fallback_theme` attribute in `self` to use as a fallback theme.

Other Parameters:

- **`theme`** (`str`) – The theme to use.
- **`custom_templates`** (`str | None`) – The path to custom templates.
- **`mdx`** (`list[str | Extension]`) – A list of Markdown extensions to use.
- **`mdx_config`** (`Mapping[str, Mapping[str, Any]]`) – Configuration for the Markdown extensions.

Methods:

- **`collect`** – Collect data given an identifier and user configuration.
- **`do_convert_markdown`** – Render Markdown text; for use inside templates.
- **`do_heading`** – Render an HTML heading and register it for the table of contents. For use inside templates.
- **`get_aliases`** – Return the possible aliases for a given identifier.
- **`get_extended_templates_dirs`** – Load template extensions for the given handler, return their templates directories.
- **`get_headings`** – Return and clear the headings gathered so far.
- **`get_inventory_urls`** – Return the URLs (and configuration options) of the inventory files to download.
- **`get_options`** – Get combined options.
- **`get_templates_dir`** – Return the path to the handler's templates directory.
- **`load_inventory`** – Yield items and their URLs from an inventory file streamed from in_file.
- **`render`** – Render a template using provided data and configuration options.
- **`render_backlinks`** – Render backlinks.
- **`teardown`** – Teardown the handler.
- **`update_env`** – Update the Jinja environment.

Attributes:

- **`custom_templates`** – The path to custom templates.
- **`domain`** (`str`) – The handler's domain, used to register objects in the inventory, for example "py".
- **`enable_inventory`** (`bool`) – Whether the inventory creation is enabled.
- **`env`** – The Jinja environment.
- **`extra_css`** (`str`) – Extra CSS.
- **`fallback_config`** (`dict`) – Fallback configuration when searching anchors for identifiers.
- **`fallback_theme`** (`str`) – Fallback theme to use when a template isn't found in the configured theme.
- **`md`** (`Markdown`) – The Markdown instance.
- **`mdx`** – The Markdown extensions to use.
- **`mdx_config`** – The configuration for the Markdown extensions.
- **`name`** (`str`) – The handler's name, for example "python".
- **`outer_layer`** (`bool`) – Whether we're in the outer Markdown conversion layer.
- **`theme`** – The selected theme.

### custom_templates

```python
custom_templates = custom_templates
```

The path to custom templates.

### domain

```python
domain: str = ''
```

The handler's domain, used to register objects in the inventory, for example "py".

### enable_inventory

```python
enable_inventory: bool = False
```

Whether the inventory creation is enabled.

### env

```python
env = Environment(
    autoescape=True,
    loader=FileSystemLoader(paths),
    auto_reload=False,
)
```

The Jinja environment.

### extra_css

```python
extra_css: str = ''
```

Extra CSS.

### fallback_config

```python
fallback_config: dict = {}
```

Fallback configuration when searching anchors for identifiers.

### fallback_theme

```python
fallback_theme: str = ''
```

Fallback theme to use when a template isn't found in the configured theme.

### md

```python
md: Markdown
```

The Markdown instance.

Raises:

- `RuntimeError` – When the Markdown instance is not set yet.

### mdx

```python
mdx = mdx
```

The Markdown extensions to use.

### mdx_config

```python
mdx_config = mdx_config
```

The configuration for the Markdown extensions.

### name

```python
name: str = ''
```

The handler's name, for example "python".

### outer_layer

```python
outer_layer: bool
```

Whether we're in the outer Markdown conversion layer.

### theme

```python
theme = theme
```

The selected theme.

### collect

```python
collect(
    identifier: str, options: HandlerOptions
) -> CollectorItem
```

Collect data given an identifier and user configuration.

In the implementation, you typically call a subprocess that returns JSON, and load that JSON again into a Python dictionary for example, though the implementation is completely free.

Parameters:

- #### **`identifier`**

  (`str`) – An identifier for which to collect data. For example, in Python, it would be 'mkdocstrings.handlers' to collect documentation about the handlers module. It can be anything that you can feed to the tool of your choice.

- #### **`options`**

  (`HandlerOptions`) – The final configuration options.

Returns:

- `CollectorItem` – Anything you want, as long as you can feed it to the handler's render method.

### do_convert_markdown

```python
do_convert_markdown(
    text: str,
    heading_level: int,
    html_id: str = "",
    *,
    strip_paragraph: bool = False,
    autoref_hook: AutorefsHookInterface | None = None,
) -> Markup
```

Render Markdown text; for use inside templates.

Parameters:

- #### **`text`**

  (`str`) – The text to convert.

- #### **`heading_level`**

  (`int`) – The base heading level to start all Markdown headings from.

- #### **`html_id`**

  (`str`, default: `''` ) – The HTML id of the element that's considered the parent of this element.

- #### **`strip_paragraph`**

  (`bool`, default: `False` ) – Whether to exclude the <p> tag from around the whole output.

Returns:

- `Markup` – An HTML string.

### do_heading

```python
do_heading(
    content: Markup,
    heading_level: int,
    *,
    role: str | None = None,
    hidden: bool = False,
    toc_label: str | None = None,
    skip_inventory: bool = False,
    **attributes: str,
) -> Markup
```

Render an HTML heading and register it for the table of contents. For use inside templates.

Parameters:

- #### **`content`**

  (`Markup`) – The HTML within the heading.

- #### **`heading_level`**

  (`int`) – The level of heading (e.g. 3 -> h3).

- #### **`role`**

  (`str | None`, default: `None` ) – An optional role for the object bound to this heading.

- #### **`hidden`**

  (`bool`, default: `False` ) – If True, only register it for the table of contents, don't render anything.

- #### **`toc_label`**

  (`str | None`, default: `None` ) – The title to use in the table of contents ('data-toc-label' attribute).

- #### **`skip_inventory`**

  (`bool`, default: `False` ) – Flag element to not be registered in the inventory (by setting a data-skip-inventory attribute).

- #### **`**attributes`**

  (`str`, default: `{}` ) – Any extra HTML attributes of the heading.

Returns:

- `Markup` – An HTML string.

### get_aliases

```python
get_aliases(identifier: str) -> tuple[str, ...]
```

Return the possible aliases for a given identifier.

Parameters:

- #### **`identifier`**

  (`str`) – The identifier to get the aliases of.

Returns:

- `tuple[str, ...]` – A tuple of strings - aliases.

### get_extended_templates_dirs

```python
get_extended_templates_dirs(handler: str) -> list[Path]
```

Load template extensions for the given handler, return their templates directories.

Parameters:

- #### **`handler`**

  (`str`) – The name of the handler to get the extended templates directory of.

Returns:

- `list[Path]` – The extensions templates directories.

### get_headings

```python
get_headings() -> Sequence[Element]
```

Return and clear the headings gathered so far.

Returns:

- `Sequence[Element]` – A list of HTML elements.

### get_inventory_urls

```python
get_inventory_urls() -> list[tuple[str, dict[str, Any]]]
```

Return the URLs (and configuration options) of the inventory files to download.

### get_options

```python
get_options(
    local_options: Mapping[str, Any],
) -> HandlerOptions
```

Get combined options.

Override this method to customize how options are combined, for example by merging the global options with the local options. By combining options here, you don't have to do it twice in `collect` and `render`.

Parameters:

- #### **`local_options`**

  (`Mapping[str, Any]`) – The local options.

Returns:

- `HandlerOptions` – The combined options.

### get_templates_dir

```python
get_templates_dir(handler: str | None = None) -> Path
```

Return the path to the handler's templates directory.

Override to customize how the templates directory is found.

Parameters:

- #### **`handler`**

  (`str | None`, default: `None` ) – The name of the handler to get the templates directory of.

Raises:

- `ModuleNotFoundError` – When no such handler is installed.
- `FileNotFoundError` – When the templates directory cannot be found.

Returns:

- `Path` – The templates directory path.

### load_inventory

```python
load_inventory(
    in_file: BinaryIO,
    url: str,
    base_url: str | None = None,
    **kwargs: Any,
) -> Iterator[tuple[str, str]]
```

Yield items and their URLs from an inventory file streamed from `in_file`.

Parameters:

- #### **`in_file`**

  (`BinaryIO`) – The binary file-like object to read the inventory from.

- #### **`url`**

  (`str`) – The URL that this file is being streamed from (used to guess base_url).

- #### **`base_url`**

  (`str | None`, default: `None` ) – The URL that this inventory's sub-paths are relative to.

- #### **`**kwargs`**

  (`Any`, default: `{}` ) – Ignore additional arguments passed from the config.

Yields:

- `tuple[str, str]` – Tuples of (item identifier, item URL).

### render

```python
render(
    data: CollectorItem,
    options: HandlerOptions,
    *,
    locale: str | None = None,
) -> str
```

Render a template using provided data and configuration options.

Parameters:

- #### **`data`**

  (`CollectorItem`) – The collected data to render.

- #### **`options`**

  (`HandlerOptions`) – The final configuration options.

- #### **`locale`**

  (`str | None`, default: `None` ) – The locale to use for translations, if any.

Returns:

- `str` – The rendered template as HTML.

### render_backlinks

```python
render_backlinks(
    backlinks: Mapping[str, Iterable[Backlink]],
    *,
    locale: str | None = None,
) -> str
```

Render backlinks.

Parameters:

- #### **`backlinks`**

  (`Mapping[str, Iterable[Backlink]]`) – A mapping of identifiers to backlinks.

- #### **`locale`**

  (`str | None`, default: `None` ) – The locale to use for translations, if any.

Returns:

- `str` – The rendered backlinks as HTML.

### teardown

```python
teardown() -> None
```

Teardown the handler.

This method should be implemented to, for example, terminate a subprocess that was started when creating the handler instance.

### update_env

```python
update_env(*args: Any, **kwargs: Any) -> None
```

Update the Jinja environment.

## CollectionError

Bases: `Exception`

An exception raised when some collection of data failed.

## Handlers

```python
Handlers(
    *,
    theme: str,
    default: str,
    inventory_project: str,
    inventory_version: str = "0.0.0",
    handlers_config: dict[str, HandlerConfig] | None = None,
    custom_templates: str | None = None,
    mdx: Sequence[str | Extension] | None = None,
    mdx_config: Mapping[str, Any] | None = None,
    locale: str = "en",
    tool_config: Any,
)
```

A collection of handlers.

Do not instantiate this directly. The plugin will keep one instance of this for the purpose of caching. Use mkdocstrings.MkdocstringsPlugin.get_handler for convenient access.

Parameters:

- ### **`theme`**

  (`str`) – The theme to use.

- ### **`default`**

  (`str`) – The default handler to use.

- ### **`inventory_project`**

  (`str`) – The project name to use in the inventory.

- ### **`inventory_version`**

  (`str`, default: `'0.0.0'` ) – The project version to use in the inventory.

- ### **`handlers_config`**

  (`dict[str, HandlerConfig] | None`, default: `None` ) – The handlers configuration.

- ### **`custom_templates`**

  (`str | None`, default: `None` ) – The path to custom templates.

- ### **`mdx`**

  (`Sequence[str | Extension] | None`, default: `None` ) – A list of Markdown extensions to use.

- ### **`mdx_config`**

  (`Mapping[str, Any] | None`, default: `None` ) – Configuration for the Markdown extensions.

- ### **`locale`**

  (`str`, default: `'en'` ) – The locale to use for translations.

- ### **`tool_config`**

  (`Any`) – Tool configuration to pass down to handlers.

Methods:

- **`get_anchors`** – Return the canonical HTML anchor for the identifier, if any of the seen handlers can collect it.
- **`get_handler`** – Get a handler thanks to its name.
- **`get_handler_config`** – Return the global configuration of the given handler.
- **`get_handler_name`** – Return the handler name defined in an "autodoc" instruction YAML configuration, or the global default handler.
- **`teardown`** – Teardown all cached handlers and clear the cache.

Attributes:

- **`inventory`** (`Inventory`) – The objects inventory.
- **`seen_handlers`** (`Iterable[BaseHandler]`) – Get the handlers that were encountered so far throughout the build.

### inventory

```python
inventory: Inventory = Inventory(
    project=inventory_project, version=inventory_version
)
```

The objects inventory.

### seen_handlers

```python
seen_handlers: Iterable[BaseHandler]
```

Get the handlers that were encountered so far throughout the build.

Returns:

- `Iterable[BaseHandler]` – An iterable of instances of BaseHandler
- `Iterable[BaseHandler]` – (usable only to loop through it).

### get_anchors

```python
get_anchors(identifier: str) -> tuple[str, ...]
```

Return the canonical HTML anchor for the identifier, if any of the seen handlers can collect it.

Parameters:

- #### **`identifier`**

  (`str`) – The identifier (one that collect can accept).

Returns:

- `tuple[str, ...]` – A tuple of strings - anchors without '#', or an empty tuple if there isn't any identifier familiar with it.

### get_handler

```python
get_handler(
    name: str, handler_config: dict | None = None
) -> BaseHandler
```

Get a handler thanks to its name.

This function dynamically imports a module named "mkdocstrings.handlers.NAME", calls its `get_handler` method to get an instance of a handler, and caches it in dictionary. It means that during one run (for each reload when serving, or once when building), a handler is instantiated only once, and reused for each "autodoc" instruction asking for it.

Parameters:

- #### **`name`**

  (`str`) – The name of the handler. Really, it's the name of the Python module holding it.

- #### **`handler_config`**

  (`dict | None`, default: `None` ) – Configuration passed to the handler.

Returns:

- `BaseHandler` – An instance of a subclass of BaseHandler, as instantiated by the get_handler method of the handler's module.

### get_handler_config

```python
get_handler_config(name: str) -> dict
```

Return the global configuration of the given handler.

Parameters:

- #### **`name`**

  (`str`) – The name of the handler to get the global configuration of.

Returns:

- `dict` – The global configuration of the given handler. It can be an empty dictionary.

### get_handler_name

```python
get_handler_name(config: dict) -> str
```

Return the handler name defined in an "autodoc" instruction YAML configuration, or the global default handler.

Parameters:

- #### **`config`**

  (`dict`) – A configuration dictionary, obtained from YAML below the "autodoc" instruction.

Returns:

- `str` – The name of the handler to use.

### teardown

```python
teardown() -> None
```

Teardown all cached handlers and clear the cache.

## HeadingShiftingTreeprocessor

```python
HeadingShiftingTreeprocessor(md: Markdown, shift_by: int)
```

Bases: `Treeprocessor`

Shift levels of all Markdown headings according to the configured base level.

Parameters:

- ### **`md`**

  (`Markdown`) – A markdown.Markdown instance.

- ### **`shift_by`**

  (`int`) – The number of heading "levels" to add to every heading.

Methods:

- **`run`** – Shift the levels of all headings in the document.

Attributes:

- **`name`** (`str`) – The name of the treeprocessor.
- **`regex`** (`Pattern`) – The regex to match heading tags.
- **`shift_by`** (`int`) – The number of heading "levels" to add to every heading. <h2> with shift_by = 3 becomes <h5>.

### name

```python
name: str = 'mkdocstrings_headings'
```

The name of the treeprocessor.

### regex

```python
regex: Pattern = compile('([Hh])([1-6])')
```

The regex to match heading tags.

### shift_by

```python
shift_by: int = shift_by
```

The number of heading "levels" to add to every heading. `<h2>` with `shift_by = 3` becomes `<h5>`.

### run

```python
run(root: Element) -> None
```

Shift the levels of all headings in the document.

## Highlighter

```python
Highlighter(md: Markdown)
```

Bases: `Highlight`

Code highlighter that tries to match the Markdown configuration.

Picking up the global config and defaults works only if you use the `codehilite` or `pymdownx.highlight` (recommended) Markdown extension.

- If you use `pymdownx.highlight`, highlighting settings are picked up from it, and the default CSS class is `.highlight`. This also means the default of `guess_lang: false`.
- Otherwise, if you use the `codehilite` extension, settings are picked up from it, and the default CSS class is `.codehilite`. Also consider setting `guess_lang: false`.
- If neither are added to `markdown_extensions`, highlighting is enabled anyway. This is for backwards compatibility. If you really want to disable highlighting even in *mkdocstrings*, add one of these extensions anyway and set `use_pygments: false`.

The underlying implementation is `pymdownx.highlight` regardless.

Parameters:

- ### **`md`**

  (`Markdown`) – The Markdown instance to read configs from.

Methods:

- **`highlight`** – Highlight a code-snippet.

### highlight

```python
highlight(
    src: str,
    language: str | None = None,
    *,
    inline: bool = False,
    dedent: bool = True,
    linenums: bool | None = None,
    **kwargs: Any,
) -> str
```

Highlight a code-snippet.

Parameters:

- #### **`src`**

  (`str`) – The code to highlight.

- #### **`language`**

  (`str | None`, default: `None` ) – Explicitly tell what language to use for highlighting.

- #### **`inline`**

  (`bool`, default: `False` ) – Whether to highlight as inline.

- #### **`dedent`**

  (`bool`, default: `True` ) – Whether to dedent the code before highlighting it or not.

- #### **`linenums`**

  (`bool | None`, default: `None` ) – Whether to add line numbers in the result.

- #### **`**kwargs`**

  (`Any`, default: `{}` ) – Pass on to pymdownx.highlight.Highlight.highlight.

Returns:

- `str` – The highlighted code as HTML text, marked safe (not escaped for HTML).

## IdPrependingTreeprocessor

```python
IdPrependingTreeprocessor(md: Markdown, id_prefix: str)
```

Bases: `Treeprocessor`

Prepend the configured prefix to IDs of all HTML elements.

Parameters:

- ### **`md`**

  (`Markdown`) – A markdown.Markdown instance.

- ### **`id_prefix`**

  (`str`) – The prefix to add to every ID. It is prepended without any separator.

Methods:

- **`run`** – Prepend the configured prefix to all IDs in the document.

Attributes:

- **`id_prefix`** (`str`) – The prefix to add to every ID. It is prepended without any separator; specify your own separator if needed.
- **`name`** (`str`) – The name of the treeprocessor.

### id_prefix

```python
id_prefix: str = id_prefix
```

The prefix to add to every ID. It is prepended without any separator; specify your own separator if needed.

### name

```python
name: str = 'mkdocstrings_ids'
```

The name of the treeprocessor.

### run

```python
run(root: Element) -> None
```

Prepend the configured prefix to all IDs in the document.

## Inventory

```python
Inventory(
    items: list[InventoryItem] | None = None,
    project: str = "project",
    version: str = "0.0.0",
)
```

Bases: `dict`

Inventory of collected and rendered objects.

Parameters:

- ### **`items`**

  (`list[InventoryItem] | None`, default: `None` ) – A list of items.

- ### **`project`**

  (`str`, default: `'project'` ) – The project name.

- ### **`version`**

  (`str`, default: `'0.0.0'` ) – The project version.

Methods:

- **`format_sphinx`** – Format this inventory as a Sphinx objects.inv file.
- **`parse_sphinx`** – Parse a Sphinx v2 inventory file and return an Inventory from it.
- **`register`** – Create and register an item.

Attributes:

- **`project`** – The project name.
- **`version`** – The project version.

### project

```python
project = project
```

The project name.

### version

```python
version = version
```

The project version.

### format_sphinx

```python
format_sphinx() -> bytes
```

Format this inventory as a Sphinx `objects.inv` file.

Returns:

- `bytes` – The inventory as bytes.

### parse_sphinx

```python
parse_sphinx(
    in_file: BinaryIO,
    *,
    domain_filter: Collection[str] = (),
) -> Inventory
```

Parse a Sphinx v2 inventory file and return an `Inventory` from it.

Parameters:

- #### **`in_file`**

  (`BinaryIO`) – The binary file-like object to read from.

- #### **`domain_filter`**

  (`Collection[str]`, default: `()` ) – A collection of domain values to allow (and filter out all other ones).

Returns:

- `Inventory` – An inventory containing the collected items.

### register

```python
register(
    name: str,
    domain: str,
    role: str,
    uri: str,
    priority: int = 1,
    dispname: str | None = None,
) -> None
```

Create and register an item.

Parameters:

- #### **`name`**

  (`str`) – The item name.

- #### **`domain`**

  (`str`) – The item domain, like 'python' or 'crystal'.

- #### **`role`**

  (`str`) – The item role, like 'class' or 'method'.

- #### **`uri`**

  (`str`) – The item URI.

- #### **`priority`**

  (`int`, default: `1` ) – The item priority. Only used internally by mkdocstrings and Sphinx.

- #### **`dispname`**

  (`str | None`, default: `None` ) – The item display name.

## InventoryItem

```python
InventoryItem(
    name: str,
    domain: str,
    role: str,
    uri: str,
    priority: int = 1,
    dispname: str | None = None,
)
```

Inventory item.

Parameters:

- ### **`name`**

  (`str`) – The item name.

- ### **`domain`**

  (`str`) – The item domain, like 'python' or 'crystal'.

- ### **`role`**

  (`str`) – The item role, like 'class' or 'method'.

- ### **`uri`**

  (`str`) – The item URI.

- ### **`priority`**

  (`int`, default: `1` ) – The item priority. Only used internally by mkdocstrings and Sphinx.

- ### **`dispname`**

  (`str | None`, default: `None` ) – The item display name.

Methods:

- **`format_sphinx`** – Format this item as a Sphinx inventory line.
- **`parse_sphinx`** – Parse a line from a Sphinx v2 inventory file and return an InventoryItem from it.

Attributes:

- **`dispname`** (`str`) – The item display name.
- **`domain`** (`str`) – The item domain.
- **`name`** (`str`) – The item name.
- **`priority`** (`int`) – The item priority.
- **`role`** (`str`) – The item role.
- **`sphinx_item_regex`** – Regex to parse a Sphinx v2 inventory line.
- **`uri`** (`str`) – The item URI.

### dispname

```python
dispname: str = dispname or name
```

The item display name.

### domain

```python
domain: str = domain
```

The item domain.

### name

```python
name: str = name
```

The item name.

### priority

```python
priority: int = priority
```

The item priority.

### role

```python
role: str = role
```

The item role.

### sphinx_item_regex

```python
sphinx_item_regex = compile(
    "^(.+?)\\s+(\\S+):(\\S+)\\s+(-?\\d+)\\s+(\\S+)\\s*(.*)$"
)
```

Regex to parse a Sphinx v2 inventory line.

### uri

```python
uri: str = uri
```

The item URI.

### format_sphinx

```python
format_sphinx() -> str
```

Format this item as a Sphinx inventory line.

Returns:

- `str` – A line formatted for an objects.inv file.

### parse_sphinx

```python
parse_sphinx(
    line: str, *, return_none: Literal[False]
) -> InventoryItem
```

```python
parse_sphinx(
    line: str, *, return_none: Literal[True]
) -> InventoryItem | None
```

```python
parse_sphinx(
    line: str, *, return_none: bool = False
) -> InventoryItem | None
```

Parse a line from a Sphinx v2 inventory file and return an `InventoryItem` from it.

## LoggerAdapter

```python
LoggerAdapter(prefix: str, logger: Logger)
```

Bases: `LoggerAdapter`

A logger adapter to prefix messages.

This adapter also adds an additional parameter to logging methods called `once`: if `True`, the message will only be logged once.

Examples:

In Python code:

```pycon
>>> logger = get_logger("myplugin")
>>> logger.debug("This is a debug message.")
>>> logger.info("This is an info message.", once=True)
```

In Jinja templates (logger available in context as `log`):

```jinja
{{ log.debug("This is a debug message.") }}
{{ log.info("This is an info message.", once=True) }}
```

Parameters:

- ### **`prefix`**

  (`str`) – The string to insert in front of every message.

- ### **`logger`**

  (`Logger`) – The logger instance.

Methods:

- **`log`** – Log a message.
- **`process`** – Process the message.

Attributes:

- **`prefix`** – The prefix to insert in front of every message.

### prefix

```python
prefix = prefix
```

The prefix to insert in front of every message.

### log

```python
log(
    level: int, msg: object, *args: object, **kwargs: object
) -> None
```

Log a message.

Parameters:

- #### **`level`**

  (`int`) – The logging level.

- #### **`msg`**

  (`object`) – The message.

- #### **`*args`**

  (`object`, default: `()` ) – Additional arguments passed to parent method.

- #### **`**kwargs`**

  (`object`, default: `{}` ) – Additional keyword arguments passed to parent method.

### process

```python
process(
    msg: str, kwargs: MutableMapping[str, Any]
) -> tuple[str, Any]
```

Process the message.

Parameters:

- #### **`msg`**

  (`str`) – The message:

- #### **`kwargs`**

  (`MutableMapping[str, Any]`) – Remaining arguments.

Returns:

- `tuple[str, Any]` – The processed message.

## MkdocstringsExtension

```python
MkdocstringsExtension(
    handlers: Handlers,
    autorefs: AutorefsPlugin,
    **kwargs: Any,
)
```

Bases: `Extension`

Our Markdown extension.

It cannot work outside of `mkdocstrings`.

Parameters:

- ### **`handlers`**

  (`Handlers`) – The handlers container.

- ### **`autorefs`**

  (`AutorefsPlugin`) – The autorefs plugin instance.

- ### **`**kwargs`**

  (`Any`, default: `{}` ) – Keyword arguments used by markdown.extensions.Extension.

Methods:

- **`extendMarkdown`** – Register the extension.

### extendMarkdown

```python
extendMarkdown(md: Markdown) -> None
```

Register the extension.

Add an instance of our AutoDocProcessor to the Markdown parser.

Parameters:

- #### **`md`**

  (`Markdown`) – A markdown.Markdown instance.

## MkdocstringsInnerExtension

```python
MkdocstringsInnerExtension(headings: list[Element])
```

Bases: `Extension`

Extension that should always be added to Markdown sub-documents that handlers request (and *only* them).

Parameters:

- ### **`headings`**

  (`list[Element]`) – A list that will be populated with all HTML heading elements encountered in the document.

Methods:

- **`extendMarkdown`** – Register the extension.

Attributes:

- **`headings`** – The list that will be populated with all HTML heading elements encountered in the document.

### headings

```python
headings = headings
```

The list that will be populated with all HTML heading elements encountered in the document.

### extendMarkdown

```python
extendMarkdown(md: Markdown) -> None
```

Register the extension.

Parameters:

- #### **`md`**

  (`Markdown`) – A markdown.Markdown instance.

## MkdocstringsPlugin

```python
MkdocstringsPlugin()
```

Bases: `BasePlugin[PluginConfig]`

An `mkdocs` plugin.

This plugin defines the following event hooks:

- `on_config`
- `on_env`
- `on_post_build`

Check the [Developing Plugins](https://www.mkdocs.org/user-guide/plugins/#developing-plugins) page of `mkdocs` for more information about its plugin system.

Methods:

- **`get_handler`** – Get a handler by its name. See mkdocstrings.Handlers.get_handler.
- **`on_config`** – Instantiate our Markdown extension.
- **`on_post_build`** – Teardown the handlers.

Attributes:

- **`css_filename`** (`str`) – The path of the CSS file to write in the site directory.
- **`handlers`** (`Handlers`) – Get the instance of mkdocstrings.Handlers for this plugin/build.
- **`inventory_enabled`** (`bool`) – Tell if the inventory is enabled or not.
- **`on_env`** – Extra actions that need to happen after all Markdown-to-HTML page rendering.
- **`plugin_enabled`** (`bool`) – Tell if the plugin is enabled or not.

### css_filename

```python
css_filename: str = 'assets/_mkdocstrings.css'
```

The path of the CSS file to write in the site directory.

### handlers

```python
handlers: Handlers
```

Get the instance of mkdocstrings.Handlers for this plugin/build.

Raises:

- `RuntimeError` – If the plugin hasn't been initialized with a config.

Returns:

- `Handlers` – An instance of mkdocstrings.Handlers (the same throughout the build).

### inventory_enabled

```python
inventory_enabled: bool
```

Tell if the inventory is enabled or not.

Returns:

- `bool` – Whether the inventory is enabled.

### on_env

```python
on_env = CombinedEvent(
    _on_env_load_inventories,
    _on_env_add_css,
    _on_env_write_inventory,
    _on_env_apply_backlinks,
)
```

Extra actions that need to happen after all Markdown-to-HTML page rendering.

Hook for the [`on_env` event](https://www.mkdocs.org/user-guide/plugins/#on_env).

- Gather results from background inventory download tasks.
- Write mkdocstrings' extra files (CSS, inventory) into the site directory.
- Apply backlinks to the HTML output of each page.

### plugin_enabled

```python
plugin_enabled: bool
```

Tell if the plugin is enabled or not.

Returns:

- `bool` – Whether the plugin is enabled.

### get_handler

```python
get_handler(handler_name: str) -> BaseHandler
```

Get a handler by its name. See mkdocstrings.Handlers.get_handler.

Parameters:

- #### **`handler_name`**

  (`str`) – The name of the handler.

Returns:

- `BaseHandler` – An instance of a subclass of BaseHandler.

### on_config

```python
on_config(config: MkDocsConfig) -> MkDocsConfig | None
```

Instantiate our Markdown extension.

Hook for the [`on_config` event](https://www.mkdocs.org/user-guide/plugins/#on_config). In this hook, we instantiate our MkdocstringsExtension and add it to the list of Markdown extensions used by `mkdocs`.

We pass this plugin's configuration dictionary to the extension when instantiating it (it will need it later when processing markdown to get handlers and their global configurations).

Parameters:

- #### **`config`**

  (`MkDocsConfig`) – The MkDocs config object.

Returns:

- `MkDocsConfig | None` – The modified config.

### on_post_build

```python
on_post_build(config: MkDocsConfig, **kwargs: Any) -> None
```

Teardown the handlers.

Hook for the [`on_post_build` event](https://www.mkdocs.org/user-guide/plugins/#on_post_build). This hook is used to teardown all the handlers that were instantiated and cached during documentation buildup.

For example, a handler could open a subprocess in the background and keep it open to feed it "autodoc" instructions and get back JSON data. If so, it should then close the subprocess at some point: the proper place to do this is in the handler's `teardown` method, which is indirectly called by this hook.

Parameters:

- #### **`config`**

  (`MkDocsConfig`) – The MkDocs config object.

- #### **`**kwargs`**

  (`Any`, default: `{}` ) – Additional arguments passed by MkDocs.

## ParagraphStrippingTreeprocessor

Bases: `Treeprocessor`

Unwraps the `<p>` element around the whole output.

Methods:

- **`run`** – Unwrap the root element if it's a single <p> element.

Attributes:

- **`name`** (`str`) – The name of the treeprocessor.
- **`strip`** (`bool`) – Whether to strip <p> elements or not.

### name

```python
name: str = 'mkdocstrings_strip_paragraph'
```

The name of the treeprocessor.

### strip

```python
strip: bool = False
```

Whether to strip `<p>` elements or not.

### run

```python
run(root: Element) -> Element | None
```

Unwrap the root element if it's a single `<p>` element.

## PluginConfig

Bases: `Config`

The configuration options of `mkdocstrings`, written in `mkdocs.yml`.

Attributes:

- **`custom_templates`** – Location of custom templates to use when rendering API objects.
- **`default_handler`** – The default handler to use. The value is the name of the handler module. Default is "python".
- **`enable_inventory`** – Whether to enable object inventory creation.
- **`enabled`** – Whether to enable the plugin. Default is true. If false, mkdocstrings will not collect or render anything.
- **`handlers`** – Global configuration of handlers.
- **`locale`** – The locale to use for translations.

### custom_templates

```python
custom_templates = Optional(Dir(exists=True))
```

Location of custom templates to use when rendering API objects.

Value should be the path of a directory relative to the MkDocs configuration file.

### default_handler

```python
default_handler = Type(str, default='python')
```

The default handler to use. The value is the name of the handler module. Default is "python".

### enable_inventory

```python
enable_inventory = Optional(Type(bool))
```

Whether to enable object inventory creation.

### enabled

```python
enabled = Type(bool, default=True)
```

Whether to enable the plugin. Default is true. If false, *mkdocstrings* will not collect or render anything.

### handlers

```python
handlers = Type(dict, default={})
```

Global configuration of handlers.

You can set global configuration per handler, applied everywhere, but overridable in each "autodoc" instruction. Example:

```yaml
plugins:
  - mkdocstrings:
      handlers:
        python:
          options:
            option1: true
            option2: "value"
        rust:
          options:
            option9: 2
```

### locale

```python
locale = Optional(Type(str))
```

The locale to use for translations.

## TemplateLogger

```python
TemplateLogger(logger: LoggerAdapter)
```

A wrapper class to allow logging in templates.

The logging methods provided by this class all accept two parameters:

- `msg`: The message to log.
- `once`: If `True`, the message will only be logged once.

Methods:

- **`debug`** – Function to log a DEBUG message.
- **`info`** – Function to log an INFO message.
- **`warning`** – Function to log a WARNING message.
- **`error`** – Function to log an ERROR message.
- **`critical`** – Function to log a CRITICAL message.

Parameters:

- ### **`logger`**

  (`LoggerAdapter`) – A logger adapter.

Attributes:

- **`critical`** – Log a CRITICAL message.
- **`debug`** – Log a DEBUG message.
- **`error`** – Log an ERROR message.
- **`info`** – Log an INFO message.
- **`warning`** – Log a WARNING message.

### critical

```python
critical = get_template_logger_function(critical)
```

Log a CRITICAL message.

### debug

```python
debug = get_template_logger_function(debug)
```

Log a DEBUG message.

### error

```python
error = get_template_logger_function(error)
```

Log an ERROR message.

### info

```python
info = get_template_logger_function(info)
```

Log an INFO message.

### warning

```python
warning = get_template_logger_function(warning)
```

Log a WARNING message.

## ThemeNotSupported

Bases: `Exception`

An exception raised to tell a theme is not supported.

## do_any

```python
do_any(seq: Sequence, attribute: str | None = None) -> bool
```

Check if at least one of the item in the sequence evaluates to true.

The `any` builtin as a filter for Jinja templates.

Parameters:

- ### **`seq`**

  (`Sequence`) – An iterable object.

- ### **`attribute`**

  (`str | None`, default: `None` ) – The attribute name to use on each object of the iterable.

Returns:

- `bool` – A boolean telling if any object of the iterable evaluated to True.

## get_logger

```python
get_logger(name: str) -> LoggerAdapter
```

Return a pre-configured logger.

Parameters:

- ### **`name`**

  (`str`) – The name to use with logging.getLogger.

Returns:

- `LoggerAdapter` – A logger configured to work well in MkDocs.

## get_template_logger

```python
get_template_logger(
    handler_name: str | None = None,
) -> TemplateLogger
```

Return a logger usable in templates.

Parameters:

- ### **`handler_name`**

  (`str | None`, default: `None` ) – The name of the handler.

Returns:

- `TemplateLogger` – A template logger.

## get_template_logger_function

```python
get_template_logger_function(
    logger_func: Callable,
) -> Callable
```

Create a wrapper function that automatically receives the Jinja template context.

Parameters:

- ### **`logger_func`**

  (`Callable`) – The logger function to use within the wrapper.

Returns:

- `Callable` – A function.

## get_template_path

```python
get_template_path(context: Context) -> str
```

Return the path to the template currently using the given context.

Parameters:

- ### **`context`**

  (`Context`) – The template context.

Returns:

- `str` – The relative path to the template.

## extension

Deprecated. Import from `mkdocstrings` directly.

## handlers

Deprecated. Import from `mkdocstrings` directly.

Modules:

- **`base`** – Deprecated. Import from mkdocstrings directly.
- **`rendering`** – Deprecated. Import from mkdocstrings directly.

### base

Deprecated. Import from `mkdocstrings` directly.

### rendering

Deprecated. Import from `mkdocstrings` directly.

## inventory

Deprecated. Import from `mkdocstrings` directly.

## loggers

Deprecated. Import from `mkdocstrings` directly.

## plugin

Deprecated. Import from `mkdocstrings` directly.
