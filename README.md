# Saigon Bistro Langenfeld — Website

Statische, mehrseitige Website für das Saigon Bistro (Langenfeld, Rheinland). Reines HTML/CSS/JS
ohne eigenen Build-Schritt und ohne Abhängigkeiten — kann direkt über GitHub Pages gehostet werden.
Einzige Ausnahme: Die Startseite (`index.html`) nutzt für die „Aktuelles"-Box Jekyll, das
GitHub-Pages-eigene, automatische Build-System (kein zusätzliches Tooling nötig, siehe unten).

## Seiten

- `index.html` — Startseite: Hero, Restaurant-Fotos, Google-Rezensionen, Bildergalerie (Slider) der
  Gerichte **und Kontaktbereich** (`#kontakt`)
- `speisekarte.html` — Vollständige, filterbare Speisekarte
- `anfahrt.html` — Anfahrtsbeschreibung mit Karte

## Lokal ansehen

`speisekarte.html` und `anfahrt.html` lassen sich weiterhin einfach direkt im Browser öffnen
(kein Server nötig).

`index.html` enthält seit der „Aktuelles"-Box Jekyll/Liquid-Syntax (`{% ... %}`), die **nur beim
GitHub-Pages-Build verarbeitet wird** — ein einfacher lokaler HTTP-Server reicht dafür nicht mehr
aus. Für eine echte Vorschau der Startseite lokal wird Jekyll benötigt:

```bash
gem install jekyll
jekyll serve
```

und dann `http://localhost:4000` öffnen. Ohne lokales Jekyll einfach direkt auf GitHub Pages
testen (push in den Branch, den Pages nutzt — siehe unten).

## Auf GitHub veröffentlichen (GitHub Pages)

1. Neues Repository auf GitHub anlegen (z. B. `saigon-bistro-website`), **ohne** README/License,
   damit es leer ist.
2. In diesem Ordner:

   ```bash
   git init
   git add .
   git commit -m "Initial website"
   git branch -M main
   git remote add origin https://github.com/<dein-benutzername>/<repo-name>.git
   git push -u origin main
   ```

3. Auf GitHub: **Settings → Pages → Build and deployment → Source: "Deploy from a branch"**,
   Branch `main`, Ordner `/ (root)` auswählen, speichern.
4. Nach 1–2 Minuten ist die Seite unter `https://<dein-benutzername>.github.io/<repo-name>/`
   erreichbar.

Wer eine eigene Domain (z. B. `saigon-bistro-langenfeld.de`) verwenden möchte, trägt sie unter
**Settings → Pages → Custom domain** ein und richtet beim Domain-Anbieter einen CNAME/A-Record
auf GitHub Pages ein.

### Als Unterseite der eigenen Homepage einbinden

Alle Links und Bild-/CSS-/JS-Pfade in diesem Projekt sind **relativ** (z. B. `href="speisekarte.html"`,
`src="images/logo.webp"`, nicht `/images/...`). Der Ordner funktioniert deshalb unverändert, egal ob er
unter der Domain-Wurzel liegt oder als Unterordner einer bestehenden Seite eingebunden wird, z. B.:

```
https://ihre-domain.de/saigon-bistro/index.html
https://ihre-domain.de/saigon-bistro/speisekarte.html
```

Einfach den kompletten `website/`-Ordner (Inhalt, nicht den Ordner selbst) per FTP/SFTP oder über das
Hosting-Panel in einen Unterordner (z. B. `saigon-bistro/`) auf dem bestehenden Webspace hochladen.

## Vor Suchmaschinen & Bots verbergen (aktuell aktiv)

Die Seite ist momentan **nicht für Suchmaschinen/Crawler bestimmt** — sie soll erst ausgewählten Kunden
per direktem Link gezeigt werden, bevor sie öffentlich auffindbar wird. Dafür ist bereits eingerichtet:

- **`<meta name="robots" content="noindex, nofollow">`** in allen drei HTML-Seiten — seriöse Suchmaschinen
  (Google, Bing, …) indexieren die Seite dadurch nicht und folgen auch keinen Links von ihr aus. Das
  funktioniert unabhängig davon, unter welchem Pfad die Seite liegt (Domain-Wurzel oder Unterordner).
- **`robots.txt`** (im Ordner enthalten) mit `Disallow: /` — das greift allerdings nur, wenn diese Datei
  am **Domain-Root** liegt (z. B. bei einem eigenen GitHub-Pages-Auftritt). Wird die Seite stattdessen als
  Unterordner in eine bestehende Homepage eingebunden, hat deren eigene, bereits vorhandene `robots.txt`
  Vorrang — in dem Fall bitte dort zusätzlich eine Zeile
  ```
  Disallow: /saigon-bistro/
  ```
  (Pfad an den tatsächlichen Unterordner anpassen) ergänzen. Die mitgelieferte `robots.txt` kann dann
  gelöscht oder ignoriert werden.
- Über einen direkten Link (z. B. per QR-Code oder E-Mail) ist die Seite trotzdem für jeden normal
  erreichbar — `noindex` blockiert nur das Auffinden über Suchmaschinen, nicht den Zugriff selbst.

**Sobald die Seite öffentlich gehen soll:** die Zeile `<meta name="robots" content="noindex, nofollow">`
in allen drei HTML-Dateien entfernen (oder auf `index, follow` ändern) und ggf. die `Disallow`-Zeile aus
der (eigenen oder eingebundenen) `robots.txt` wieder streichen.

## „Aktuelles"-Hinweisbox auf der Startseite

Auf der Startseite (`index.html`) erscheint ganz oben (vor dem Hero-Bereich) eine Hinweisbox für
kurzfristige Mitteilungen (z. B. Betriebsferien, Feiertagsschließung). Der Text steht **nicht** im
HTML, sondern in der einfachen Textdatei `aktuelles.txt` im Hauptordner:

- **Ist `aktuelles.txt` leer** (oder nicht vorhanden), wird die Box beim Seiten-Build gar nicht erst
  ins HTML geschrieben.
- **Steht Text in `aktuelles.txt`**, erscheint er automatisch in der Box — kein HTML/CSS-Wissen
  nötig.

Um eine Mitteilung zu setzen oder zu ändern: Datei `aktuelles.txt` direkt auf GitHub öffnen (Stift-Symbol
„Edit this file"), Text eintragen (z. B. `Wir sind bis zum 25. Oktober in Betriebsferien. Danach sind
wir wieder für Sie da.`) und committen. Um die Mitteilung wieder auszublenden, einfach den gesamten
Inhalt der Datei löschen und mit leerer Datei committen.

Technisch: Es kommt **kein JavaScript** zum Einsatz. GitHub Pages baut die Seite standardmäßig mit
Jekyll (kein `.nojekyll` im Repo, keine gesonderte Konfiguration nötig). `index.html` trägt dafür
einen minimalen Jekyll-„Front Matter"-Block (`--- layout: null ---`) am Dateianfang, damit GitHub
Pages die Datei durch den Liquid-Templating-Prozessor schickt. Ein Liquid-Block liest `aktuelles.txt`
über `include_relative` ein, entfernt Leerzeichen/Zeilenumbrüche und rendert die Box nur, wenn danach
noch Text übrig ist:

```liquid
{% capture aktuelles_raw %}{% include_relative aktuelles.txt %}{% endcapture %}
{% assign aktuelles_message = aktuelles_raw | strip %}
{% if aktuelles_message != "" %}
  ... Box mit {{ aktuelles_message | escape }} ...
{% endif %}
```

Das Rendering passiert also vollständig serverseitig beim GitHub-Pages-Build, bevor die Seite an
den Browser ausgeliefert wird — nicht mehr im Browser per `fetch`. Alle anderen Seiten
(`speisekarte.html`, `anfahrt.html`, …) bleiben unverändert reine, von Jekyll unangetastete
HTML-Dateien, da nur `index.html` einen Front-Matter-Block besitzt.

## Inhalte, die noch ergänzt werden sollten

- Die Speisekarten-Preise wurden aus den gescannten Menükarten übertragen; bei mehreren Gerichten
  mit gleicher „wahlweise dazu"-Auswahl (Hühnerfleisch/Rind/Ente/Garnelen/Gemüse/Tofu/vegane
  Sesam-Ente) wurde die auf der Karte wiederkehrende Standard-Preistabelle verwendet. Bitte vor
  Veröffentlichung mit der aktuellen Karte gegenprüfen.

## Struktur

```
website/
├── index.html
├── speisekarte.html
├── anfahrt.html
├── robots.txt        (nur wirksam, wenn am Domain-Root gehostet — siehe oben)
├── css/style.css
├── js/main.js
└── images/
    ├── logo.webp
    ├── restaurant-1.webp / restaurant-2.webp   (Innenraum)
    └── dish-*.webp                             (Gerichte, auch im Startseiten-Slider)
```

## Technik

- Kein Framework, kein Build-Schritt — reines HTML/CSS/JS, läuft auf jedem Static-Hosting
  (GitHub Pages, Netlify, eigener Webspace …).
- Schriften: [Fraunces](https://fonts.google.com/specimen/Fraunces) (Überschriften) und
  [Be Vietnam Pro](https://fonts.google.com/specimen/Be+Vietnam+Pro) (Fließtext), von Google Fonts
  eingebunden.
- Kartenanzeige über den offiziellen [OpenStreetMap](https://www.openstreetmap.org)-Iframe-Embed
  (kein API-Key, keine Google-Cookies/-Drittanbieterübertragung — daher auch kein separater
  Datenschutz-Hinweis für die Karte nötig). Koordinaten wurden per OSM-Nominatim für „Zum Stadion 71,
  40764 Langenfeld" ermittelt.
