# MeteoGate Documentation – Development Branch (`docs-dev`)

This branch is used for writing, updating, and reviewing documentation for **MeteoGate**. All documentation work should be done here before publishing.

> Changes from this branch are reviewed and then merged into the `gh-pages` branch, which is published via GitHub Pages.

---

## 🚀 How to Contribute

1. **Clone the repository** and switch to this branch:
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

5. **Open a pull request from `docs-dev` to `gh-pages` to publish.**

After merging the pull request, the documentation will be available at:  
📍 **https://eumetnet.github.io/meteogate-documentation/**

---

## 📁 Documentation Structure

All documentation source files are located in the `docs/` directory, organized by section:

- `index.md` – Homepage
- `1-overview.md`, `2-discovering-and-accessing-data.md`, etc. – Section content
- `glossary.md`, `references.md` – Supporting material

---

## 🛠️ Tools

- **MkDocs** – Static site generator for documentation
- **Material for MkDocs** – Theme used for styling and navigation
- **GitHub Pages** – Used for publishing the site from the `gh-pages` branch
- **Read the Docs** *(optional)* – Alternative platform for documentation hosting

---

ℹ️ Need help? Let a maintainer know or open an issue!
