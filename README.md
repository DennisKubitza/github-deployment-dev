# Saigon Bistro Langenfeld — Website

Statische, mehrseitige Website für das Saigon Bistro (Langenfeld, Rheinland). Reines HTML/CSS/JS
ohne eigenen Build-Schritt und ohne Abhängigkeiten — kann direkt über GitHub Pages gehostet werden.
Ausnahme: `index.html`, `speisekarte.html`, `mittagsangebote.html` und `anfahrt.html` nutzen
Jekyll, das GitHub-Pages-eigene, automatische Build-System, um bearbeitbare Inhalte (Hinweisbox,
Preise, …) aus einfachen Textdateien ins HTML einzusetzen — kein zusätzliches Tooling nötig, siehe
unten.

## Seiten

- `index.html` — Startseite: Hero, Restaurant-Fotos, Google-Rezensionen, Bildergalerie (Slider) der
  Gerichte **und Kontaktbereich** (`#kontakt`)
- `speisekarte.html` — Vollständige, filterbare Speisekarte
- `mittagsangebote.html` — Mittagstisch-Menü (M1–M8)
- `anfahrt.html` — Anfahrtsbeschreibung mit Karte

## Lokal ansehen

`impressum.html`, `datenschutz.html` und `404.html` lassen sich weiterhin einfach direkt im Browser
öffnen (kein Server nötig).

`index.html`, `speisekarte.html`, `mittagsangebote.html` und `anfahrt.html` enthalten
Jekyll/Liquid-Syntax (`{% ... %}`), die **nur beim GitHub-Pages-Build verarbeitet wird** — ein
einfacher lokaler HTTP-Server reicht dafür nicht mehr aus, und öffnet man die Datei direkt, sieht
man die rohen `{{ ... }}`-Platzhalter statt der Preise/Texte. Für eine echte Vorschau lokal wird
Jekyll benötigt:

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
| `content/aktuelles.txt` | Hinweisbox ganz oben auf **allen vier Hauptseiten** (Startseite, Speisekarte, Mittagsmenü, Anfahrt) für kurzfristige Mitteilungen (Betriebsferien, Feiertagsschließung). Eine Datei steuert die Box auf allen vier Seiten gleichzeitig. **Leer = Box wird nirgends angezeigt.** | `Wir sind bis zum 25. Oktober in Betriebsferien. Danach sind wir wieder für Sie da.` |
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
Browser ausgeliefert wird. Die „Aktuelles"-Box selbst ist in `index.html`, `speisekarte.html`,
`mittagsangebote.html` und `anfahrt.html` identisch eingebettet (gleicher Liquid-Block, gleicher
Aufruf von `content/aktuelles.txt`) — eine Änderung der Datei wirkt sich dadurch auf allen vier
Seiten gleichzeitig aus. Nur `impressum.html`, `datenschutz.html` und `404.html` bleiben
unverändert reine, von Jekyll unangetastete HTML-Dateien ohne Front-Matter-Block.

## Speisekarten-Preise (`_data/`-Ordner)

Alle Preise in `speisekarte.html` (151 Stück) und `mittagsangebote.html` (8 Stück) stehen **nicht**
im HTML, sondern in zwei Listen-Dateien:

- `_data/preise_speisekarte.yml`
- `_data/preise_mittagsangebote.yml`

Jede Zeile ist ein **eigenständiger, unabhängiger Preis** — auch wenn mehrere Gerichte gerade
denselben Betrag haben (z. B. viele Gerichte mit „Hühnerfleisch 12,00€"), sind das in der Liste
absichtlich separate Zeilen. Einen Preis für **ein** Gericht zu ändern, ändert also **nie**
versehentlich den Preis eines anderen Gerichts.

```yaml
"1": 3,50€
"2-Huehnerfleisch": 4,50€
"20-A": 12,00€
"20-B": 13,00€
```

**Bearbeiten:** Datei auf GitHub öffnen, nur den Betrag **nach dem Doppelpunkt** ändern (z. B.
`12,00€` → `13,00€`), committen. Die Bezeichnung davor in Anführungszeichen bitte nicht anfassen —
sie verbindet die Zeile mit dem richtigen Gericht auf der Seite (Gerichtnummer wie auf der Karte
gedruckt, bei Gerichten mit Auswahl zusätzlich ein Buchstabe/Kürzel für die jeweilige Variante,
z. B. `20-A` = Gericht 20, Variante A).

**Wird ein ganzes Gericht aus `speisekarte.html` gelöscht:** einfach den ganzen
`<article class="dish">…</article>`-Block löschen. Die übrigen Gerichte sind davon nicht betroffen
— jede Preis-Zeile ist über ihre feste Bezeichnung mit genau einem Gericht verknüpft, nicht über
ihre Position in der Liste. Eine jetzt unbenutzte Zeile in `_data/preise_speisekarte.yml` bleibt
einfach ungenutzt liegen (kein Fehler, keine Auswirkung) und kann bei Gelegenheit mit entfernt
werden, muss aber nicht.

**Fehlt eine Preis-Zeile** (z. B. neues Gericht in der HTML-Datei angelegt, aber die passende
Zeile in `_data/preise_speisekarte.yml` vergessen oder deren Bezeichnung falsch abgetippt), zeigt
die Seite an der Stelle sichtbar **„Preis auf Anfrage"** an, statt einfach leer zu bleiben — das
macht einen vergessenen Preis sofort auf der Live-Seite auffindbar, anstatt lautlos zu verschwinden.

### Technisch

`_data/` ist ein von Jekyll fest vorgesehener Ordner für genau diesen Zweck (Nachschlage-Daten) —
im Unterschied zu den Dateien in `content/` (die nur eingefügt, aber nicht durchsucht werden
müssen) braucht es hier einen echten Zugriff „gib mir den Preis für Schlüssel 20-A", und das kann
Jekyll nur über `_data/` nativ und ohne eigenen Code. In `speisekarte.html`/`mittagsangebote.html`
sieht der Zugriff so aus:

```liquid
<div class="dish-price">{{ site.data.preise_speisekarte["1"] | default: "Preis auf Anfrage" }}</div>
...
<span>{{ site.data.preise_speisekarte["20-A"] | default: "Preis auf Anfrage" }}</span>
```

Beide Dateien wurden automatisiert aus dem vorherigen, fest im HTML stehenden Preisstand erzeugt
und danach Zeile für Zeile gegen das Original geprüft — es wurde kein Preis beim Umbau verändert.

## Inhalte, die noch ergänzt werden sollten

- Die Speisekarten-Preise wurden ursprünglich aus den gescannten Menükarten übertragen; bitte
  einmalig mit der aktuellen Karte gegenprüfen (jetzt einfach möglich über die beiden Dateien in
  `_data/`, siehe oben).
- `speisekarte.html` und `mittagsangebote.html` zeigen in ihren `schema.org`-Bewertungsdaten
  jeweils fest `"reviewCount": "41"`, während `index.html` (über `content/google-bewertung.txt`)
  aktuell `44` zeigt — ein vorbestehender, veralteter Wert auf beiden Menüseiten, unabhängig vom
  heutigen Preis-Umbau. Beim nächsten Update der Bewertungsanzahl bitte auch dort mit pflegen.

## Struktur

```
website/
├── index.html
├── speisekarte.html
├── mittagsangebote.html
├── anfahrt.html
├── robots.txt        (nur wirksam, wenn am Domain-Root gehostet — siehe oben)
├── content/           (bearbeitbare Textbausteine, siehe „Bearbeitbare Inhalte" oben)
│   ├── aktuelles.txt
│   ├── abholung.txt
│   ├── lieferung.txt
│   ├── oeffnungszeiten.txt
│   └── google-bewertung.txt
├── _data/              (Speisekarten-Preise, siehe „Speisekarten-Preise" oben)
│   ├── preise_speisekarte.yml
│   └── preise_mittagsangebote.yml
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
