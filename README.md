# MeteoGate Documentation Repository

This is the repository for **MeteoGate documentation and instructions**. It contains user, operator, and developer documentation related to the MeteoGate system and its components.

## Branch Structure

This repository uses a **branch-based workflow** for managing and publishing documentation:

- **`docs-dev`** – Development branch  
  All updates and changes to the documentation are made in this branch. Use pull requests to propose changes, which are reviewed and approved before being merged.

- **`gh-pages`** – Published branch  
  This branch contains the rendered documentation and is used by GitHub Pages to serve the published site. **Do not edit this branch directly.**

## Workflow

1. Clone the repository and switch to the `docs-dev` branch:
   ```bash
   git clone https://github.com/<org-or-user>/<repo>.git
   cd <repo>
   git checkout docs-dev
2. Make changes in the Markdown files in the docs/ folder.
3. Preview the site locally (requires MkDocs).
   ```bash
    mkdocs serve
4. Commit and push changes:
   ```bash
    git add .
    git commit -m "Update docs"
    git push origin docs-dev
5. Open a pull request from docs-dev to gh-pages for publishing.

## Documentation Structure

Documentation source files are located in the docs/ directory, organized by section:

index.md: Homepage

- 1-overview.md, 2-discovering-and-accessing-data.md, etc.: Section content
- glossary.md, references.md: Supporting material

## Tools

- MkDocs – Static site generator
- Material for MkDocs – Theme used for styling the documentation
- GitHub Pages – Static site hosting used to publish the documentation
- Read the Docs (optional) – Hosting alternative (not currently used in production)
