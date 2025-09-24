# Documentation project for Eclipse Arrowhead 5th generation
**java-spring reference implementation**

Built by Material for MkDocs

- https://squidfunk.github.io/mkdocs-material/setup/
- https://www.mkdocs.org/

## Requirements
- python
- mkdocs-material | `pip install mkdocs-material`

## Commands

- start dev server: `mkdocs serve`

## Release

**Versioning**
- PATCH: Corrections on existing resources
- MINOR: Creations of new pages
- MAJOR: New Core/Support system docs due to new AH release or structural changes of the site

## Deploy

- run: `mkdocs gh-deploy`
  - It builds the site and pushes the site files into the `gh-pages` branch. (This branch should not be edited manually, it exists only for the deployment)
