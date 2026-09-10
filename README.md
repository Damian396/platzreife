# Platzreife-App aufs Handy bringen

Fünf Dateien, ein Repository, danach liegt die App als Icon auf dem Home-Bildschirm — offline nutzbar, Fortschritt bleibt gespeichert.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | Die App selbst |
| `manifest.webmanifest` | Name, Icon, Farbe, Vollbildmodus |
| `sw.js` | Service Worker: macht die App offline verfügbar |
| `icon-192.png`, `icon-512.png` | App-Icon |

Alle fünf müssen im **selben Ordner** liegen.

## Schritt für Schritt

1. Auf GitHub ein Repository anlegen, zum Beispiel `platzreife`. Sichtbarkeit: public.
2. Alle fünf Dateien hochladen (Add file → Upload files → Commit).
3. Settings → Pages → Source: `Deploy from a branch`, Branch: `main`, Ordner `/ (root)` → Save.
4. Ein bis zwei Minuten warten. Die Adresse lautet dann
   `https://DEINNAME.github.io/platzreife/`
5. Die Adresse auf dem Handy im Browser öffnen.

## Installieren

**iPhone (Safari):** Teilen-Symbol → «Zum Home-Bildschirm» → Hinzufügen.
Wichtig: das geht nur in Safari, nicht in Chrome.

**Android (Chrome):** Menü (drei Punkte) → «App installieren» oder «Zum Startbildschirm hinzufügen».

Danach startet die App im Vollbild ohne Browserleiste, funktioniert ohne Internet und speichert den Fortschritt dauerhaft.

## Wichtig zum Speichern

- Der Fortschritt liegt im Speicher des Browsers, mit dem du installiert hast. Immer dasselbe Icon benutzen, nicht mal Safari, mal Chrome.
- Der Fortschritt ist pro Gerät. Handy und Laptop führen getrennte Stände.
- Browserdaten oder Website-Daten löschen setzt den Fortschritt zurück.
- Wenn du später eine neue Version hochlädst: in `sw.js` die Zeile `const CACHE = "platzreife-v1";` auf `v2` ändern, sonst zeigt das Handy weiter die alte Version.
