---
id: 44fda0b1-9034-4924-a29b-d1823b800756
title: "Kjente problemer og løsninger"
linkTitle: "Kjente problemer"
weight: 80
---

Kronologisk logg over feil og problemer oppdaget og løst under utvikling av SAMT-BU Docs. For hvert problem dokumenteres symptom, rotårsak og løsning – slik at det er lettere å diagnostisere lignende problemer i fremtiden.

---

## ✅ LØST 2026-03-03: Scrollbarer synlige + scroll-fade borte + ytelsesforsinkelse

**Symptomer:**
- Venstre kolonne (#sidebar): synlig scrollbar, scroll-fade (gradient) virket ikke
- Midtpanel (#body): uventet scrollbar synlig, stor tom sone under innhold
- Ytelse: merkbar forsinkelse

**Rotårsak:** `altinninfoportal.js`, `altinndocs.js` og `altinndocs-learn.js` bruker alle jQuery (`$()`) direkte – uten `$(document).ready()`. De ble lastet synkront (uten `defer`), men jQuery hadde `defer`. Resultatet: `$` er undefined → alle tre scripts feiler. Spesielt kritisk: `altinndocs.js` linje 2 kjørte `$('.js-moveChildrenFrom').insertAfter('.js-moveChildrenTo')` øyeblikkelig – DOM-manipulasjon mislyktes → stor tom sone under innhold.

`custom-footer.html` brukte `$(document).ready(...)` for dropdown-togglerne (språk, Innhold, Endre). Inline scripts kan ikke ha `defer` → `$` undefined → dropdowns virket ikke.

**Fix:**
1. `footer.html`: Lagt til `defer` på `altinninfoportal.js`, `altinndocs.js`, `altinndocs-learn.js`
2. `custom-footer.html`: Konvertert fra jQuery til vanilla JS (IIFE med direkte DOM-tilgang)

**Lærdom:** Når jQuery er `defer`, MÅ alle jQuery-avhengige scripts enten ha `defer` (externe filer) eller bruke vanilla JS (inline scripts). Sjekk ALLTID altinn-scripts og custom-footer.html ved jQuery-relaterte endringer.

---

## 2026-03-02: Søk virket ikke (jQuery defer-konflikt)

**Symptom:** Søkefeltet i headeren aksepterte input, men viste ingen resultater.

**Rotårsak:** jQuery ble lastet med `defer` i `header.html` (innført 2026-02-26 for ytelsesoptimalisering). `search.js` og avhengighetene `lunr.min.js` og `horsey.js` ble derimot lastet synkront i `<body>`. Nettleseren kjørte `search.js` umiddelbart under sideparsing – på det tidspunktet var `$` (jQuery) ikke tilgjengelig ennå. Kallet `$.getJSON(...)` kastet en feil, `lunrIndex` forble `null`, og alle søk returnerte tomt resultat uten synlig feilmelding.

**Fix:** `defer`-attributt lagt til på alle tre search-scripts i `layouts/partials/search.html` (temaet). Med `defer` garanterer nettleseren utføringsrekkefølge basert på dokumentrekkefølge:

1. jQuery (`header.html`, defer)
2. `lunr.min.js` (`search.html`, defer)
3. `horsey.js` (`search.html`, defer)
4. `search.js` (`search.html`, defer)

Inline `<script>var baseurl = "...";</script>` kjøres fortsatt synkront – korrekt, siden variabelen settes under parsing og er tilgjengelig når de deferred scriptene kjøres.

**Filer endret:** `themes/hugo-theme-samt-bu/layouts/partials/search.html`

**Lærdom:** Når ett script lastes med `defer`, må *alle* scripts som avhenger av det også ha `defer`.

---

## 2026-02-xx: Sidebar-ikon ute av sync med CSS

**Symptom:** Aktiv side viste `fa-caret-right` (lukket) i stedet for `fa-sort-down` (åpen) i sidebaren.

**Rotårsak:** `menu.html` manglet betingelsen `eq .RelPermalink $currentNode.RelPermalink` i ikonlogikken. `IsAncestor` er bare `true` for *forfedre* til gjeldende side, ikke for selve siden. På aktiv side var ingen betingelse oppfylt → feil ikon vises, men CSS viser siden som aktiv (ute av sync).

**Fix:** Lagt til `eq .RelPermalink $currentNode.RelPermalink` i betingelsen i `menu.html`. Korrekt logikk: `fa-sort-down` vises når `IsAncestor` ELLER `eq .RelPermalink` ELLER `alwaysopen`.

**Filer endret:** `themes/hugo-theme-samt-bu/layouts/partials/menu.html`

---

## Prosedyre: GitHub Pages stuck deploy

Av og til havner GitHub Pages i en tilstand der deployer kjøres uten effekt (siden ser ut til å deploye, men endringer dukker ikke opp).

**Fremgangsmåte for reset:**

```bash
"/c/Program Files/GitHub CLI/gh.exe" api -X DELETE repos/SAMT-BU/samt-bu-docs/pages
"/c/Program Files/GitHub CLI/gh.exe" api -X POST repos/SAMT-BU/samt-bu-docs/pages \
  --field build_type=workflow \
  --field source[branch]=main \
  --field source[path]="/"
git commit --allow-empty -m "Reset Pages"
git push
```

---

## Windows: `git mv` feiler for mapper med mange filer

**Symptom:** `git mv gammel-mappe/ ny-mappe/` feiler med feilmelding om at kildemappen ikke finnes.

**Workaround:**

```bash
cp -r gammel-mappe/ ny-mappe/
git rm -r gammel-mappe/
git add ny-mappe/
```
