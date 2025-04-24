# MeteoGate Documentation

This repository contains the documentation for the **MeteoGate** system.

MeteoGate provides a one-stop shop for discovering, accessing, and sharing meteorological and hydrological data.

## Branches in this Repository

- `docs-dev` — Development branch for editing and writing documentation (Markdown).
- `main` — Integration branch for reviewed and accepted changes.
- `gh-pages` — Built from `main` and published via GitHub Pages.

## Published Documentation Site

🔗 https://eumetnet.github.io/meteogate-documentation/

## Want to contribute?

Documentation is written, updated and reviewed in [`docs-dev`](https://github.com/eumetnet/meteogate-documentation/tree/docs-dev) branch.  All documentation work should be done in this branch before publishing.

> Changes from this branch are reviewed and merged into the `main` branch by a pull request. The documentation is then built and published from `main` to the `gh-pages` branch via MkDocs and GitHub Actions.

---

## How to Contribute

1. **Clone the repository** and switch to the `docs-dev` branch:
   ```bash
   git clone https://github.com/<org-or-user>/<repo>.git
   cd <repo>
   git checkout docs-dev
   ```

2. **Edit or create Markdown files** in the `docs/` directory.

3. **Preview the documentation locally** using MkDocs:
   ```bash
   mkdocs serve
   ```
   Then open `http://127.0.0.1:8000` in your browser.

4. **Commit and push your changes**:
   ```bash
   git add .
   git commit -m "Describe your update"
   git push origin docs-dev
   ```

5. **Open a pull request from `docs-dev` to `main`**  
   This allows review before the changes are published.

6. **Once merged into `main`, the site is built and deployed** to the `gh-pages` branch.  
 
The live site is available at:  
🔗 https://eumetnet.github.io/meteogate-documentation/

---

## Documentation Structure

All documentation source files are located in the `docs/` directory, organized by section:

- `index.md` – Homepage
- `1-overview.md`, `2-discovering-and-accessing-data.md`, etc. – Section content
- `glossary.md`, `references.md` – Supporting material

---

## Tools

- **MkDocs** – Static site generator for documentation
- **Material for MkDocs** – Theme used for styling and navigation
- **GitHub Pages** – Used for publishing the site from the `gh-pages` branch
- **Read the Docs** *(optional)* – Alternative platform for documentation hosting

---

ℹ️ **Need help?** Let a maintainer know or open an issue!
