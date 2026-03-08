---
id: 36ddabdf-4ceb-45bb-b55f-051cd9976078
title: "Legg til underkapittel – valg i Endre-menyen"
linkTitle: "Legg til underkapittel"
weight: 10
status: "Tidlig utkast"
---

**Behov:** Når man leser en side i nettstedet, er det ingen enkel vei til å opprette et nytt underkapittel under denne siden. «Endre»-menyen i headeren tilbyr i dag «Denne siden» (deep-link til CMS-editor) og «Andre valg» (CMS-portal). Et tredje valg – «Legg til underkapittel» – ville dekke et naturlig neste steg i redigeringsarbeidsflyten.

## Foreslått løsning

En dynamisk lenke i `edit-switcher.html` som åpner GitHubs «create file»-grensesnitt på riktig sti:

```
https://github.com/<org>/<repo>/new/main/content/<sti-til-gjeldende-side>/
```

GitHub åpner da et tomt filredigeringsvindu der brukeren skriver filnavnet (f.eks. `nytt-kapittel/_index.nb.md`) og frontmatter. Lenken bygges dynamisk fra `.File.Path` i Hugo-templaten, på samme måte som eksisterende deep-link til Decap CMS.

**Fordeler fremfor Decap CMS-alternativet:**

- Decap CMS «New»-lenke oppretter alltid på rotnivå i samlingen – brukeren må navigere manuelt til riktig mappe
- GitHub-lenken setter brukeren direkte i riktig katalog

## Gjenstående arbeid

- Legge til nytt menyvalg i `edit-switcher.html` (temaet) for alle fire portalbetingelser
- Teste at GitHub-lenken fungerer korrekt for modul-sider (der repo-et er et annet enn `samt-bu-docs`)
- Vurdere om valget skal vises på alle sider, eller bare der det gir mening
- Tilsvarende «Add sub-chapter»-valg for engelsk meny
