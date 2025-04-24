# MeteoGate Documentation

This repository contains the documentation for the **MeteoGate** system.

📚 Documentation is **developed** in the `docs-dev` branch.  
📦 Documentation is **reviewed and merged** into `main`.  
🌐 The documentation is **built from `main` and published** via `gh-pages`.

---

## 🔄 Branch Workflow

- `docs-dev` — Development branch for editing and writing documentation (Markdown).
- `main` — Integration branch for reviewed and accepted changes.
- `gh-pages` — Built automatically from `main` and published via GitHub Pages.

---

## ✍️ How to Contribute

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

## 🛠 Tools Used

- [MkDocs](https://www.mkdocs.org/) – Static site generator
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) – Theme
- [GitHub Pages](https://pages.github.com/) – Hosting
