# jsidech.github.io

This repository contains the source for Jonah Sider-Echenberg's personal
blog, built with Quarto. Posts are written in a mix
of Python and R.

## Prerequisites

Install these before cloning:

- **Quarto** — v1.10.18 or later
- **uv** — v0.12.15 or later 
- **R** — 4.6.1 or later

`renv` (R's package manager) does **not** need to be installed separately —
it's declared as part of this project and bootstraps itself automatically
the first time `renv::restore()` is run below.

## Build instructions

Run these in order. Each block is labeled with where it runs.

**1. Clone the repository** (terminal)
```sh
git clone https://github.com/jsidech/jsidech.github.io.git
cd jsidech.github.io
```

**2. Install Python dependencies** (terminal, from the repo root)
```sh
uv sync
```
This reads `pyproject.toml` / `uv.lock` and creates a local `.venv` with
the exact package versions used to write this site.

**3. Install R dependencies** (R console, started from the repo root)
```r
renv::restore()
```
This reads `renv.lock` and installs the exact R package versions used to
write this site into a project-local library. Start R from the repo root
(or open the project in RStudio via its `.Rproj` file, if present) so renv
activates automatically.

**4. Render the site** (terminal, from the repo root)
```sh
uv run quarto render
```
Using `uv run` (rather than calling `quarto` directly) matters here: `uv sync`
does not add `.venv` to your system `PATH`, so a bare `quarto render` will
fail to find the project's Python packages and error out looking for a
system-wide `python3`. `uv run` temporarily activates the project's `.venv`
for just this command, which is what lets Quarto's Python engine find
`jupyter` and the packages installed in step 2.

## Viewing the built site

Quarto writes the built site to the `docs/` folder at the repo root. You
can either:

- Open `docs/index.html` directly in a browser, or
- Run a live-reloading local preview instead of a one-off render:
  ```sh
  uv run quarto preview
  ```
  This serves the site locally (Quarto will print the URL, typically
  `http://localhost:4200` or similar, and open it automatically) and
  rebuilds pages as you edit them.

## Data source and network access

All posts use the [Palmer Penguins](https://allisonhorst.github.io/palmerpenguins/)
dataset, collected by Dr. Kristen Gorman at the Palmer Station Long Term
Ecological Research Network in Antarctica. The data ships bundled inside
the `palmerpenguins` package itself (both the R and Python versions), so
**no network access is required at render time** to fetch data — rendering
runs entirely offline once steps 2 and 3 have completed. Network access is
only needed once, during `uv sync` and `renv::restore()`, to download the
packages themselves.