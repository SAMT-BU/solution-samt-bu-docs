---
title: "TipTap som WYSIWYG-editor"
linkTitle: "TipTap som editor"
weight: 40
status: "Ny"
---

Vurdere TipTap som erstatning for Quill v1 i «Rediger innhold»-dialogen (qe-dialog).

## Bakgrunn

«Rediger innhold»-dialogen (`edit-switcher.html` + `custom-footer.html`) bruker Quill v1.3.7. Etter forsøk på å integrere `quill-better-table` for visuell tabelleditor, konkluderte vi med at:

- `quill-better-table` er fundamentalt inkompatibel med `position:fixed`-containere (DOM-feil ved konstruktør)
- Quill v1 rendrer tabeller som rå GFM-tekst, ikke visuelt
- Quill v1 er ikke lenger aktivt vedlikeholdt

## Hva vi forventer av TipTap

| Krav | Kommentar |
|------|-----------|
| Tabeller visuelt | TipTap `@tiptap/extension-table` + `@tiptap/extension-table-row` etc. |
| Markdown in/out | `@tiptap/extension-markdown` (community) eller manuell serialisering |
| CDN-lasting uten bundler | ESM-import fra jsDelivr eller unpkg |
| Paste av bilder | Custom paste-handler (samme mønster som nåværende Quill-implementasjon) |
| Headings, lister, lenker | Standard TipTap-extensions |

## Kritiske avklaringer før implementasjon

1. **CDN vs. bundler:** TipTap er primært designet for bundler-miljøer (Vite/webpack). Sjekk om ESM-CDN-import fungerer uten byggesteg i en `<script>`-tag i `edit-switcher.html` / `custom-footer.html`.
2. **Markdown-serialisering:** TipTap lagrer som JSON internt. Konvertering til Markdown (for GitHub-commit) krever enten `@tiptap/extension-markdown` eller Turndown på HTML-output (`.getHTML()`).
3. **CSS-scope:** Quill-CSS er scopet til `.ql-*`-klasser. TipTap bruker `.ProseMirror`. Sørg for at tema-CSS ikke kolliderer med TipTap-stiler.
4. **Paste-bilder:** Implementer tilsvarende paste-handler som i Quill-versjonen – base64, `qeImages`/`qeImageMap`, commit som separate filer.

## Tilbakevending til Quill – hva som gjenstår

Hvis TipTap ikke egner seg, og vi går tilbake til Quill:

- Paste av bilder i qe-dialog er implementert men **ikke testet i produksjon** – verifiser full flyt (paste → base64 → commit)
- Tabell-insert via GFM-mal er den eneste realistiske løsningen (quill-better-table er utelukket)

## Relatert

- `edit-switcher.html`: qe-dialog HTML og CSS
- `custom-footer.html`: `loadLibs`, `initQeEditor`, `qeImages`/`qeImageMap`, paste-handler
- Endringslogg 2026-03-11 ettermiddag i `claude-kontekst/`
