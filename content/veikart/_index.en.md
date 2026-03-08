---
id: 50be39b6-7d99-4048-bf9e-a8a85a31410c
title: "Roadmap"
linkTitle: "Roadmap"
weight: 20
---

Planned improvements and extensions to the SAMT-BU Docs platform. Each item describes the need, proposed solution, and remaining work.

---

## ◔ Add sub-chapter – option in the Edit menu

**Need:** When reading a page on the site, there is no easy way to create a new sub-chapter under the current page. The «Edit» menu in the header currently offers «This page» (deep-link to the CMS editor) and «Other options» (CMS portal). A third option – «Add sub-chapter» – would cover a natural next step in the editorial workflow.

**Proposed solution:** A dynamic link in `edit-switcher.html` that opens GitHub's «create file» interface at the correct path:

```
https://github.com/<org>/<repo>/new/main/content/<path-to-current-page>/
```

GitHub opens an empty file editor where the user types the filename (e.g. `new-chapter/_index.en.md`) and frontmatter. The link is built dynamically from `.File.Path` in the Hugo template, in the same way as the existing deep-link to Decap CMS.

**Advantages over the Decap CMS alternative:**
- Decap CMS «New» link always creates at the root level of the collection – the user must navigate manually to the correct folder
- The GitHub link places the user directly in the correct directory

**Remaining work:**
- Add new menu option in `edit-switcher.html` (the theme) for all four portal conditions
- Test that the GitHub link works correctly for module pages (where the repository differs from `samt-bu-docs`)
- Consider whether the option should appear on all pages, or only where it makes sense
- Corresponding «Legg til underkapittel» option for the Norwegian menu

---

## ○ Creating a new module repository – step by step

Guide for creating a new Hugo module repository from scratch, from empty repo to mounted content in the site. Covers: `hugo mod init`, content structure, `go.mod` registration, CI setup with cross-repo triggering, and CMS portal.

*Not started.*

---

## ○ Adding a new CMS portal

Template and checklist for creating nb + en CMS portals: folder structure, `config.yml` template, registration in the portal overview (`static/edit/index.html`), and new branch in `edit-switcher.html`.

*Not started.*

---

## ○ Helper scripts

Tools that simplify common operations: initialising a new module repository, creating a new page with correct frontmatter template, synchronising module pointers. Can be implemented as shell scripts, Python scripts, or GitHub Actions.

*Not started.*
