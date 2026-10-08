---
name: cp7-projekt
description: Fügt eine fertige HTML-Seite als neues Projekt zur Website cp7.online / cp7.beer hinzu (Ordner anlegen, in projects.json eintragen, committen, pushen). Verwenden, wenn der Nutzer eine HTML-Datei oder HTML-Code "hochladen", "veröffentlichen", "zur Seite hinzufügen" oder "als Projekt eintragen" möchte, oder ein bestehendes Projekt aktualisieren will.
---

# Wichtig: Wo gearbeitet wird

Dieser Skill liegt im Repo `cpracher-cloud/cp7.online`, bearbeitet aber das Repo **`cpracher-cloud/cp7.beer`** (privat). Dort liegt die Website, und Cloudflare Pages deployt aus ihm. Das Repo `cp7.online` selbst enthält keine Seite.

- Ist `cp7.beer` im Arbeitsverzeichnis vorhanden, dort arbeiten.
- Sonst `gh repo clone cpracher-cloud/cp7.beer` bzw. `git clone https://github.com/cpracher-cloud/cp7.beer.git` versuchen. Gelingt das nicht (kein Zugriff), dem Nutzer sagen, dass die Sitzung `cp7.beer` verbunden haben muss, und stattdessen die Weboberfläche-Variante am Ende nennen. Niemals die Projektdateien in `cp7.online` ablegen.
- Alle Pfade unten (`projects/`, `projects.json`, `new-project.mjs`, `_headers`) beziehen sich auf `cp7.beer`.

# Neues Projekt auf cp7.online veröffentlichen

Das Repo ist eine statische Seite (GitHub `cpracher-cloud/cp7.beer`, Branch `main`). Cloudflare Pages deployt jeden Push automatisch, es gibt keinen Build-Schritt. Die Seite läuft unter cp7.online, cp7.beer und jeweils www.

## Struktur

- `index.html` und `style.css`: Startseite. Sie liest `projects.json` und zeigt die 5 neuesten Projekte (nach `date` absteigend).
- `projects/<slug>/index.html`: ein Projekt, erreichbar unter `/projects/<slug>/`.
- `projects.json`: Liste mit `slug`, `title`, `description`, `date` (YYYY-MM-DD).
- `new-project.mjs`: Skript, das Ordner und Listeneintrag anlegt.
- `_headers`: setzt `X-Robots-Tag: noindex, nofollow` für alle Pfade. Die Seiten sollen vorerst nicht von Suchmaschinen gefunden werden. Das nicht ändern, außer der Nutzer verlangt es.

## Ablauf

1. **Angaben klären**, falls nicht genannt: Titel, Kurzbeschreibung (ein Satz) und ein Ordnername (`slug`: nur Kleinbuchstaben, Ziffern, Bindestriche, z. B. `edinburgh-2026`). Den Slug aus dem Titel ableiten, wenn der Nutzer keinen vorgibt. Beschreibungen nie erfinden, sondern fragen oder leer lassen.
2. **Projekt anlegen:** `node new-project.mjs <slug> "<Titel>" "<Beschreibung>"` im Repo-Root. Das Skript erzeugt `projects/<slug>/index.html` (Platzhalter) und trägt das heutige Datum ein. Existiert der Slug schon, aktualisiert es nur den Eintrag und das Datum, der Seiteninhalt bleibt.
3. **Inhalt einsetzen:** Den Platzhalter in `projects/<slug>/index.html` durch die HTML-Datei des Nutzers ersetzen. Den Inhalt nicht umschreiben oder umgestalten. Eine liegende Datei wie `Mein Bericht.html` wird nach `projects/<slug>/index.html` verschoben, nicht in `projects/` belassen (Leerzeichen und `+` in URLs vermeiden).
4. **Kopf prüfen** und nur bei Fehlen ergänzen. Ohne diese Angaben erscheinen kaputte Umlaute und Emojis, und die Seite ist auf dem Handy nicht responsiv:
   - `<!doctype html>` und `<html lang="de">`
   - `<meta charset="utf-8">`
   - `<meta name="viewport" content="width=device-width, initial-scale=1">`
   - `<head>` und `<body>` korrekt geöffnet und geschlossen
   Prüfen mit `grep -ci charset`, `grep -ci viewport`, `grep -ci doctype`.
5. **Vorschau (optional, empfohlen bei neuem Design):** lokalen Server starten (`python3 -m http.server 8765`), `/projects/<slug>/` ansehen, danach den Server wieder beenden. Eine `file://`-Ansicht ist unzuverlässig, weil Pfade und Stylesheets nicht geladen werden.
6. **Datenschutz erwähnen:** Die Seiten sind nur per noindex ausgeblendet, aber für jeden mit Link erreichbar. Enthält die Seite Privates (Reisedaten, Buchungsnummern, Kanzleiinterna), den Nutzer darauf hinweisen. Ein Passwortschutz geht über Cloudflare Access (Zero Trust).
7. **Committen und pushen:** `git add -A`, kurze Commit-Nachricht (z. B. "Add project <titel>"), `git push`. Schlägt der Push mit "fetch first" fehl, zuerst `git pull --rebase origin main`.
8. **Rückmeldung:** Die Adresse nennen, `https://cp7.online/projects/<slug>/`, und sagen, dass der Build bei Cloudflare ein bis zwei Minuten dauert.

## Design-Skills (nur wenn Design neu gebaut oder überarbeitet wird)

Liefert der Nutzer eine fertige HTML-Datei, wird sie unverändert eingesetzt (Schritt 3). Soll eine Seite neu gestaltet, überarbeitet oder verbessert werden, vorher die passenden Skills mit dem Skill-Tool laden und deren Anweisungen befolgen:

- `html-schriften`: immer für die Schriftauswahl auf neuen oder überarbeiteten Seiten.
- `impeccable`: Gestaltung, Kritik, Politur, Layout, Typografie, Farbe, Responsivität, Barrierefreiheit.
- `ui-ux-pro-max`: Stile, Farbpaletten, Schriftpaarungen und UX-Richtlinien nachschlagen.
- `interaction-design`: Mikrointeraktionen, Übergänge, Animationen, Lade- und Feedback-Zustände.
- `shadcn`: nur wenn das Projekt shadcn/ui-Komponenten nutzt (z. B. eine `components.json` vorhanden ist). Die Projektseiten sind statisches HTML ohne Build, daher sonst nicht einsetzen.

Auch dann gilt: Kopf-Prüfung (Schritt 4) und `noindex` bleiben bestehen.

## Projekt entfernen

Ordner `projects/<slug>` löschen und den Eintrag aus `projects.json` streichen, danach committen und pushen. Das ist eine Löschung, daher vorher den Nutzer fragen.

## Ohne lokales Git (Hinweis für den Nutzer)

Per GitHub-Weboberfläche: unter `projects` mit "Add file → Create new file" `<slug>/index.html` anlegen, Code einfügen und committen. Danach `projects.json` mit dem Stiftsymbol bearbeiten und einen Eintrag samt Komma einfügen.
