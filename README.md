# VLA Framework documentation

Standalone MkDocs documentation for the sibling `vla-framework` project.
Documentation source lives in `docs/`; navigation and theme settings live in
`mkdocs.yml`. Edit these files to update the site.

## Local preview

Run from this directory using Pixi:

```bash
pixi run serve
```

Open <http://127.0.0.1:8000/vla-framework-docs/>. Saved Markdown changes reload automatically.
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

## GitHub Pages

Published site: <https://acl-lab-robotgroup.github.io/vla-framework-docs/>.

The `.github/workflows/pages.yml` workflow builds the documentation with Pixi
and deploys `site/` to GitHub Pages whenever commits are pushed to `main`.
It can also be started manually from the repository's **Actions** tab.

In the repository's **Settings → Pages**, set **Build and deployment → Source**
to **GitHub Actions**. Check **Actions → Deploy documentation** for build and
deployment status.

The canonical URL in `mkdocs.yml` includes `/vla-framework-docs/`, so the local
preview serves the documentation under that same path. The preview listens on
all network interfaces as configured in the `serve` Pixi task.

Keep the application repository and this documentation repository separate:

```text
~/Documents/
├── vla-framework/
└── vla-framework-docs/
```
