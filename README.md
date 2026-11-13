# VLA Framework documentation

Standalone MkDocs documentation for the sibling `vla-framework` project.
Documentation source lives in `docs/`; navigation and theme settings live in
`mkdocs.yml`. Edit these files to update the site.

## Local preview

Run from this directory using Pixi:

```bash
pixi run serve
```

Open <http://0.0.0.0:8000/>. Saved Markdown changes reload automatically.
Press Ctrl+C in the server terminal to stop the preview.

To install the environment from a fresh checkout:

```bash
pixi install --locked
```

Dependencies and preview/build tasks are defined in `pixi.toml`. Commit
`pixi.lock` alongside it to reproduce the environment. Pixi manages the local
environment in `.pixi/`, which is ignored by Git.

## Build and validate

```bash
pixi run build
```

The generated static site is written to `site/`, which is ignored by Git.

## Publishing later

This is a local Git repository with no remote or automatic deployment configured.
The preview binds to the local machine only.

When ready, create a separate GitHub repository named `vla-framework-docs`,
connect it as the remote, and configure a GitHub Actions workflow to build and
deploy the `site/` directory to GitHub Pages. Set `site_url` in `mkdocs.yml` to
the eventual documentation URL at that point. Review the documentation content
before making that repository public.

Keep the application repository and this documentation repository separate:

```text
~/Documents/
├── vla-framework/
└── vla-framework-docs/
```
