# ACAP Missing Features Documentation

This repository powers the public documentation portal for the ACAP Missing Features suite. The content is written in Markdown and published via **MkDocs Material** so that operational teams and integrators can explore features, installation steps, and the deep Flow reference without touching code.

## Getting started

1. Install the dependencies:
   ```bash
   pip install mkdocs-material
   ```

2. Serve locally while you write:
   ```bash
   mkdocs serve
   ```

3. Build the site for publishing:
   ```bash
   mkdocs build
   ```

The generated site lives in `site/` and mirrors the navigation defined in `mkdocs.yml`. Assets such as videos or screenshots are stored under `docs/flow`.

## Highlights

- **Full Flow Reference**: An end-user friendly breakdown of every Flow control, setting, and node (see `docs/flow/reference.md`).
- **Feature-specific guides**: Includes in-depth coverage of Flow nodes, Image to SD Card, Metadata Dispatch, Notifications, Occupancy counting, Parking Times, and the License Plate List.
- **Changelog & support**: Keep track of release notes and links to support resources via the top-level `Changelog`, `Compatibility`, and `Support` pages.

## Publishing

Documentation is hosted on GitHub Pages. After merging changes:

1. Run `mkdocs build`.
2. Commit the updated `site/` (if you publish from this repo) or push to the doc site workflow consuming the `site/` bundle.
3. Tag releases in the changelog to keep the version history accurate.
