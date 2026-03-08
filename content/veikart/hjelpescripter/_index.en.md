---
title: "Helper scripts"
linkTitle: "Helper scripts"
weight: 40
status: "New"
---

**Need:** Several common operations (initialising a new module repository, creating a new page with the correct frontmatter template, synchronising module pointers) are done manually today and are error-prone. Helper scripts will reduce friction and ensure consistency.

## Planned content

Candidate scripts:

- **New module repo:** Initialises folder structure, `hugo mod init`, `_index.nb.md`/`_index.en.md` template with correct frontmatter
- **New page:** Creates folder and both language files with UUID placeholder, title, weight, and empty content
- **Sync module pointer:** Equivalent to `hugo mod get @latest` + commit of `go.mod`/`go.sum` for one or all module repos

Implementation form to be decided: shell script, Python script, or GitHub Actions workflow.

*Not started.*
