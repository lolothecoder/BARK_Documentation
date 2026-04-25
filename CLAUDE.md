# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

Sphinx documentation site for the EPFL AI Team's **BARK** project — a student robotics group working on three quadruped robots: the **DINGO**, the **Pupper V3**, and a custom dog called **BYTE**. The site is published on Read the Docs at https://bark-documentation.readthedocs.io/. Content is prose-only reStructuredText with embedded images/videos; there is no application code to run or test.

Note: `lumache.py` and `pyproject.toml` at the repo root are vestigial leftovers from the Read the Docs tutorial template this repo was forked from. They are not part of the site.

## Build commands

All build commands run from `docs/`:

```bash
# one-time setup
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt

# build the HTML site into docs/build/html
make html

# clean rebuild (use after conf.py, toctree, or asset changes)
make clean html
```

Read the Docs builds automatically from `.readthedocs.yaml` (Python 3.10, Ubuntu 22.04, Sphinx config at `docs/source/conf.py`). There are no tests or linters — Sphinx warnings during `make html` are the only signal that something is wrong.

## Content architecture

Everything authored lives under `docs/source/`. The top-level `index.rst` is the landing page and declares all `toctree` entries that form the sidebar. Three content trees hang off it:

- **`information/`** — evergreen reference pages (safety, software setup, per-robot docs, events). Filenames are numbered (`0_safety.rst`, `1_DINGO_Software_setup.rst`, …) but the number is ordering-only; `index.rst` lists each file explicitly in the toctree. Adding a new info page requires **both** creating the `.rst` file and adding its path to the relevant toctree in `index.rst`.
- **`meetings/`** and **`work_sessions/`** — semesterly logs, organized as `{area}/{Semester_Year}/{NN_DD-MM-YYYY}.rst`. Each semester folder has a `0_index.rst` that uses `:glob: *` to auto-include every sibling `.rst`. This means new weekly entries are picked up automatically — **do not** hand-edit the semester index when adding a week. When starting a new semester, create the folder, add a `0_index.rst` mirroring the existing ones, and add it to the parent `0_index.rst`.
- **`assets/`** — images and videos referenced from `.rst` files via paths like `/assets/<subdir>/file.ext` (leading slash is the Sphinx source root). The `sphinxcontrib-video` extension (see `conf.py`) enables the `.. video::` directive used throughout weekly logs.

`generated/` is Sphinx autosummary output — do not edit by hand.

## Writing conventions

- Weekly session/meeting entries follow a consistent shape: top-level `==` heading `Week of DD.MM.YYYY`, then bold sub-sections (`**Team Software**`, `**Team Elect**`, `**Team Mecha**`, `**Miscellaneous**`) as bullet lists with inline `.. image::` / `.. video::` directives. Match existing files in the same semester folder when adding a new week.
- Filename dates are `NN_DD-MM-YYYY.rst` (two-digit week prefix for sort order, European date format). The prefix is what keeps weeks ordered under the `:glob:` toctree.
- Theme is `sphinx_rtd_theme`; the `sphinxcontrib.video` extension is required for videos to render.
