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

## Wer hat was gelernt — Admin-Ansicht

Jede Person, die die App öffnet, trägt beim ersten Start ihren Namen ein. Danach läuft alles wie bisher lokal auf dem Gerät. Zusätzlich wird — wenn Firebase eingerichtet ist (siehe unten) — eine Fortschritts­zusammenfassung (keine einzelnen Antworten, nur Aufgaben erledigt, Karten pro Box/Thema, bestes Quizresultat) in eine Cloud-Datenbank geschrieben.

Diese Übersicht kannst du unter `deine-adresse/index.html#admin` aufrufen (kleiner "⚙ Admin"-Link oben rechts im Header). Dort meldest du dich mit einem eigenen E-Mail/Passwort-Login an und siehst alle Nutzer, sortiert nach Fortschritt. Ohne dieses Login sieht niemand die Daten anderer — jede Person kann in der Datenbank technisch nur ihren eigenen Datensatz lesen und schreiben.

### Firebase einrichten (einmalig, kostenlos)

1. Auf [console.firebase.google.com](https://console.firebase.google.com) ein neues Projekt anlegen (Google-Analytics kann deaktiviert bleiben).
2. Im Projekt: **Build → Firestore Database → Datenbank erstellen** (Modus "Produktion", Standort egal, z. B. `eur3`).
3. **Build → Authentication → Sign-in method**: **E-Mail/Passwort** aktivieren. Danach unter **Users** einen Nutzer für dich selbst anlegen (deine E-Mail + ein Passwort) — das ist dein Admin-Login.
4. **Authentication → Sign-in method**: zusätzlich **Anonym** aktivieren (damit jedes Gerät automatisch und ohne Login einen eigenen Fortschritts-Datensatz bekommt).
5. Projekteinstellungen (Zahnrad oben links) → **Meine Apps** → **Web-App hinzufügen** (`</>`-Symbol). Einen beliebigen Namen vergeben, kein Hosting nötig. Du bekommst ein `firebaseConfig`-Objekt.
6. Dieses Objekt in `index.html` einsetzen — suche nach `const firebaseConfig=` und ersetze die `"DEIN_..."`-Platzhalter mit deinen echten Werten.
7. In **Firestore Database → Regeln** folgende Regeln einfügen und veröffentlichen (ersetze die E-Mail mit deiner eigenen Admin-E-Mail aus Schritt 3):

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{uid} {
         allow get, create, update: if request.auth != null && request.auth.uid == uid;
         allow get, list: if request.auth != null && request.auth.token.email == 'DEINE-ADMIN-EMAIL@beispiel.ch';
       }
     }
   }
   ```

8. Änderungen committen und pushen (oder direkt in GitHub hochladen). Ab jetzt synchronisiert jedes Gerät automatisch, und unter `#admin` siehst nur du die Gesamtübersicht.

Ohne Schritt 1–7 läuft die App unverändert wie vorher — nur eben ohne Cloud-Sync und ohne Admin-Ansicht (die Seite zeigt dann einen entsprechenden Hinweis).
