---
id: 50be39b6-7d99-4048-bf9e-a8a85a31410c
title: "Veikart"
linkTitle: "Veikart"
weight: 20
---

Planlagte forbedringer og utvidelser av SAMT-BU Docs-plattformen. Hvert punkt beskriver behov, foreslått løsning og gjenstående arbeid.

---

## ◔ Legg til underkapittel – valg i Endre-menyen

**Behov:** Når man leser en side i nettstedet, er det ingen enkel vei til å opprette et nytt underkapittel under denne siden. «Endre»-menyen i headeren tilbyr i dag «Denne siden» (deep-link til CMS-editor) og «Andre valg» (CMS-portal). Et tredje valg – «Legg til underkapittel» – ville dekke et naturlig neste steg i redigeringsarbeidsflyten.

**Foreslått løsning:** En dynamisk lenke i `edit-switcher.html` som åpner GitHubs «create file»-grensesnitt på riktig sti:

```
https://github.com/<org>/<repo>/new/main/content/<sti-til-gjeldende-side>/
```

GitHub åpner da et tomt filredigeringsvindu der brukeren skriver filnavnet (f.eks. `nytt-kapittel/_index.nb.md`) og frontmatter. Lenken bygges dynamisk fra `.File.Path` i Hugo-templaten, på samme måte som eksisterende deep-link til Decap CMS.

**Fordeler fremfor Decap CMS-alternativet:**
- Decap CMS «New»-lenke oppretter alltid på rotnivå i samlingen – brukeren må navigere manuelt til riktig mappe
- GitHub-lenken setter brukeren direkte i riktig katalog

**Gjenstående arbeid:**
- Legge til nytt menyvalg i `edit-switcher.html` (temaet) for alle fire portalbetingelser
- Teste at GitHub-lenken fungerer korrekt for modul-sider (der repo-et er et annet enn `samt-bu-docs`)
- Vurdere om valget skal vises på alle sider, eller bare der det gir mening
- Tilsvarende «Add sub-chapter»-valg for engelsk meny

---

## ○ Opprette nytt modulrepo – steg-for-steg

Veiledning for å opprette et nytt Hugo-modulrepo fra bunnen, fra tomt repo til montert innhold i nettstedet. Dekker: `hugo mod init`, innholdsstruktur, `go.mod`-registrering, CI-oppsett med kryssrepo-triggering og CMS-portal.

*Ikke påbegynt.*

---

## ○ Legge til ny CMS-portal

Mal og sjekkliste for å opprette nb + en CMS-portal: mappestruktur, `config.yml`-mal, registrering i portaloversikten (`static/edit/index.html`), og nytt grein i `edit-switcher.html`.

*Ikke påbegynt.*

---

## ○ Hjelpescripter

Verktøy som forenkler vanlige operasjoner: initialisering av nytt modulrepo, oppretting av ny side med korrekt frontmatter-mal, synkronisering av modulpekere. Kan implementeres som shell-script, Python-script eller GitHub Actions.

*Ikke påbegynt.*
