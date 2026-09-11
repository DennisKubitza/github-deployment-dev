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

## Bearbeitbare Inhalte (`content/`-Ordner)

Ein paar Textbausteine auf der Startseite stehen **nicht** im HTML, sondern als einfache Textdateien
im Ordner `content/` — damit sie sich ohne HTML/CSS-Kenntnisse direkt auf GitHub bearbeiten lassen
(Datei öffnen → Stift-Symbol „Edit this file" → Text ändern → committen). Kein JavaScript beteiligt:
GitHub Pages rendert diese Dateien beim Jekyll-Build serverseitig in `index.html` ein (siehe
„Technisch" unten).

| Datei | Wofür | Beispielinhalt |
|---|---|---|
| `content/aktuelles.txt` | Hinweisbox ganz oben auf der Startseite für kurzfristige Mitteilungen (Betriebsferien, Feiertagsschließung). **Leer = Box wird nicht angezeigt.** | `Wir sind bis zum 25. Oktober in Betriebsferien. Danach sind wir wieder für Sie da.` |
| `content/abholung.txt` | Abholzeiten im Lieferservice-Banner | `12:15–20:45 Uhr` |
| `content/lieferung.txt` | Lieferzeiten im Lieferservice-Banner | `ab 14:00 Uhr (wochentags), ab 12:15 Uhr (Wochenende)` |
| `content/oeffnungszeiten.txt` | Öffnungszeiten-Banner | `Dienstag – Freitag: 11:00 – 21:00 Uhr · Samstag & Sonntag: 12:00 – 21:00 Uhr · Montag: Ruhetag` |
| `content/google-bewertung.txt` | Google-Bewertung: **Zeile 1** = Punktzahl (mit Punkt, z. B. `4.8`), **Zeile 2** = Anzahl Bewertungen (z. B. `44`). Wird an allen drei Stellen der Startseite verwendet (Trust-Bar oben, Rezensionen-Bereich, `schema.org`-Bewertungsdaten für Suchmaschinen) — eine Änderung hier aktualisiert automatisch alle drei. | `4.8`⏎`44` |

Bei `abholung.txt` / `lieferung.txt` / `oeffnungszeiten.txt` nur den reinen Text eintragen, **ohne**
die fette Überschrift davor (z. B. nur `12:15–20:45 Uhr`, nicht `Abholung: 12:15–20:45 Uhr`) — die
Überschrift steht fest im HTML.

Bei `google-bewertung.txt` bitte die Punktzahl immer mit **Punkt** (`4.9`, nicht `4,9`) eintragen —
das deutsche Komma-Format für die Anzeige wird automatisch daraus erzeugt, während `schema.org`
den Punkt als gültiges Zahlenformat benötigt.

**Hinweis:** Die `schema.org`-Öffnungszeiten (`openingHoursSpecification`, strukturierte Daten im
`<head>` für Google) sind separat als einzelne Wochentag/Uhrzeit-Felder hinterlegt und werden
**nicht** automatisch aus `content/oeffnungszeiten.txt` befüllt — bei einer echten Änderung der
Öffnungszeiten also beide Stellen pflegen (Text in `content/oeffnungszeiten.txt` **und** die
`openingHoursSpecification` weiter oben in `index.html`).

### Technisch

Es kommt **kein JavaScript** zum Einsatz. GitHub Pages baut die Seite standardmäßig mit Jekyll
(kein `.nojekyll` im Repo, keine gesonderte Konfiguration nötig). `index.html` trägt dafür einen
minimalen Jekyll-„Front Matter"-Block (`--- layout: null ---`) am Dateianfang, damit GitHub Pages
die Datei durch den Liquid-Templating-Prozessor schickt. Liquid-Blöcke lesen die Dateien aus
`content/` über `include_relative` ein und setzen den Text direkt ins HTML — z. B. für die
Hinweisbox, deren Rendering zusätzlich davon abhängt, ob nach dem Entfernen von
Leerzeichen/Zeilenumbrüchen noch Text übrig ist:

```liquid
{% capture aktuelles_raw %}{% include_relative content/aktuelles.txt %}{% endcapture %}
{% assign aktuelles_message = aktuelles_raw | strip %}
{% if aktuelles_message != "" %}
  ... Box mit {{ aktuelles_message | escape }} ...
{% endif %}
```

Die Google-Bewertung liegt als zwei Zeilen in einer Datei; da Liquids `split`-Filter nicht direkt auf
echte Zeilenumbrüche matcht, wird dafür der gängige Jekyll-Kniff verwendet, einen Zeilenumbruch per
`capture` in eine Variable zu holen:

```liquid
{% capture newline %}
{% endcapture %}
{% capture google_bewertung_raw %}{% include_relative content/google-bewertung.txt %}{% endcapture %}
{% assign google_bewertung_zeilen = google_bewertung_raw | strip | split: newline %}
{% assign google_score = google_bewertung_zeilen[0] | strip %}
{% assign google_score_de = google_score | replace: ".", "," %}
{% assign google_anzahl = google_bewertung_zeilen[1] | strip %}
```

Das Rendering passiert vollständig serverseitig beim GitHub-Pages-Build, bevor die Seite an den
Browser ausgeliefert wird. Alle anderen Seiten (`speisekarte.html`, `anfahrt.html`, …) bleiben
unverändert reine, von Jekyll unangetastete HTML-Dateien, da nur `index.html` einen
Front-Matter-Block besitzt.

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
├── content/           (bearbeitbare Textbausteine, siehe „Bearbeitbare Inhalte" oben)
│   ├── aktuelles.txt
│   ├── abholung.txt
│   ├── lieferung.txt
│   ├── oeffnungszeiten.txt
│   └── google-bewertung.txt
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
