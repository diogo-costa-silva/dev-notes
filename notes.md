# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.


## Link

https://diogo-costa-silva.github.io/dev-notes/



## Commands

```bash
uv init
uv venv
source .venv/bin/activate
````

```bash
uv add mkdocs-material pymdown-extensions
uv add mkdocs-awesome-pages-plugin mkdocs-section-index

uv run mkdocs new .
```

```bash

git submodule add https://github.com/diogo-costa-silva/dev-templates.git docs/dev-templates
git submodule update --init --recursive


uv run mkdocs build

```

```bash

uv run mkdocs serve


```
