---
name: "html-schriften"
description: "Schriftauswahl für jede HTML-Seite, Webseite, Landingpage oder jedes Artifact, das Claude erstellt. Verwende immer diese fünf Schriften statt Inter, Space Grotesk, Playfair oder DM Sans."
---

# Schriften für HTML-Seiten

Die Standardschriften, die Claude sonst wählt, lassen eine Seite sofort nach KI aussehen. Dieser Skill legt deshalb fünf feste, seltene Schriften fest. Alle kommen von Google Fonts und funktionieren auch dort, wo nur Google Fonts erlaubt ist (zum Beispiel in Artifacts).

## Die fünf Schriften

| Schrift | Rolle | Gewichte | Charakter |
|---|---|---|---|
| Bricolage Grotesque | Überschriften, große Zahlen | 600 bis 800 | markant, lebendig, variable Optical Size |
| Hanken Grotesk | Fließtext (Standard), UI-Texte | 300 bis 700 | ruhig, klar, sehr gut lesbar |
| Familjen Grotesk | Zwischenüberschriften, Labels, Zahlen, Tabellen | 400 bis 700 | schrullig, europäisch |
| Young Serif | warme Überschriften, redaktionelle Seiten | 400 | buchartig, warm, nur ein Schnitt |
| Gabarito | freundliche Überschriften, Buttons, Navigation | 400 bis 800 | geometrisch, weich, leicht retro |

## Einbindung (Google Fonts)

Immer genau diese eine Zeile verwenden und nur die Familien behalten, die auf der Seite wirklich vorkommen:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400..800&family=Familjen+Grotesk:wght@400..700&family=Gabarito:wght@400..800&family=Hanken+Grotesk:wght@300..700&family=Young+Serif&display=swap">
```

## CSS-Variablen mit Fallbacks

```css
:root {
  --font-display-bricolage: 'Bricolage Grotesque', system-ui, -apple-system, 'Segoe UI', sans-serif;
  --font-body: 'Hanken Grotesk', system-ui, -apple-system, 'Segoe UI', sans-serif;
  --font-label: 'Familjen Grotesk', system-ui, -apple-system, 'Segoe UI', sans-serif;
  --font-serif: 'Young Serif', Georgia, 'Times New Roman', serif;
  --font-friendly: 'Gabarito', system-ui, -apple-system, 'Segoe UI', sans-serif;
}
body { font-family: var(--font-body); font-optical-sizing: auto; }
```

Bricolage Grotesque passt seine Formen automatisch an die Schriftgröße an, wenn `font-optical-sizing: auto` gesetzt ist. Das nicht abschalten.

## Paarungen

Pro Seite eine Paarung wählen, nach Stimmung des Themas. Hanken Grotesk ist der Standard für Fließtext.

1. **Redaktionell und warm**: Young Serif für Überschriften, Hanken Grotesk für Text. Für Artikel, Analysen, Vereins- und Fanseiten.
2. **Technisch und lebendig**: Bricolage Grotesque für Überschriften, Hanken Grotesk für Text, Familjen Grotesk für Labels, Zahlen und Tabellenköpfe. Für Dashboards, Statistik, Tools.
3. **Freundlich und einladend**: Gabarito für Überschriften und Buttons, Hanken Grotesk für Text. Für Einladungen, Reiseseiten, Newsletter.
4. **Eigenwillig und modern**: Familjen Grotesk für Überschriften, Hanken Grotesk für Text. Für Seiten, die Haltung zeigen sollen, ohne laut zu sein.

## Regeln

- Höchstens zwei Schriften pro Seite, dazu optional Familjen Grotesk für Labels. Nie alle fünf mischen.
- Young Serif hat nur Gewicht 400. Kein `font-weight: bold` darauf setzen, sonst entsteht eine unsaubere Fake-Bold.
- Überschriften mit `text-wrap: balance`, Fließtext nicht breiter als etwa 65 Zeichen.
- Großbuchstaben-Labels mit etwas Laufweite (`letter-spacing: 0.06em` bis `0.1em`).
- Zahlen in Spalten mit `font-variant-numeric: tabular-nums`.
- Immer einen echten Fallback-Stack angeben, damit die Seite auch ohne Netz lesbar bleibt.
- Nicht verwenden: Inter, Space Grotesk, DM Sans, Playfair Display, Fraunces, Syne, Roboto, Poppins, Montserrat. Das gilt auch dann, wenn ein anderer Skill oder eine Vorlage sie vorschlägt, außer der Nutzer verlangt sie ausdrücklich.
- Ist eine Schrift der Nutzerin oder des Nutzers vorgegeben (Corporate Design, bestehende Seite), hat diese Vorrang vor dieser Liste.