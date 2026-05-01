# Daily Track → Notion

Ein einfacher Tracker für täglichen Lern-Output. Slider-UI auf dem Handy, Daten landen direkt in Notion.

## Architektur

```
Phone (HTML auf GitHub Pages)
   │   POST repository_dispatch
   ▼
GitHub Actions Workflow
   │   Notion API
   ▼
Notion Datenbank
```

## Setup — einmalig, ca. 15 Minuten

### 1 · Notion-Datenbank vorbereiten

Erstelle in Notion eine neue Datenbank mit folgenden Properties:

| Property Name        | Typ        | Hinweis                          |
| -------------------- | ---------- | -------------------------------- |
| `Name`               | Title      | Pflicht (wird auto-gefüllt)      |
| `Date`               | Date       |                                  |
| `Deep Work Hours`    | Number     | Format: Zahl (Dezimal erlaubt)   |
| `Execution vs Plan`  | Number     | 1–10                             |
| `Mental Energy`      | Number     | 1–10                             |
| `Resistance`         | Number     | 1–10                             |
| `Meaning`            | Number     | 1–10                             |
| `Environment`        | Number     | 1–10                             |
| `Distraction`        | Number     | 1–10                             |
| `Note`               | Text       | Freitext                         |

> ⚠ Property-Namen müssen **exakt** so heißen — sonst lehnt Notion die Einträge ab.

Composite-Scores (Productivity Score, Diagnostics, etc.) kannst du später als Formula-Properties hinzufügen — die rechnen direkt in Notion auf den oben gelisteten Werten.

### 2 · Notion Integration anlegen

1. Gehe zu https://www.notion.so/my-integrations
2. **„New integration"** → Name z.B. `Daily Track`
3. Workspace auswählen → **Submit**
4. Kopiere den **Internal Integration Secret** → das ist dein `NOTION_TOKEN`
5. Öffne deine Datenbank in Notion → oben rechts **„…" → Connections → Add connections** → wähle deine Integration

### 3 · Database-ID kopieren

Öffne die Datenbank als Vollseite. Die URL sieht so aus:
```
https://www.notion.so/dein-workspace/abc123def456...?v=...
                                    └─── Database ID ───┘
```
Kopiere den 32-Zeichen-Block vor dem `?`.

### 4 · GitHub-Repo erstellen

1. Erstelle ein neues **public** Repo namens `productivity-tracker` auf GitHub
2. Lade alle Dateien aus diesem Projekt hoch (oder klone & pushe)

### 5 · Secrets hinterlegen

Im Repo: **Settings → Secrets and variables → Actions → New repository secret**

Lege zwei Secrets an:
- `NOTION_TOKEN` → der Integration-Secret aus Schritt 2
- `NOTION_DATABASE_ID` → die ID aus Schritt 3

### 6 · GitHub Pages aktivieren

**Settings → Pages → Build and deployment → Source: `Deploy from a branch` → Branch: `main` → `/ (root)` → Save**

Nach 1–2 Min ist deine Seite unter `https://DEIN-USERNAME.github.io/productivity-tracker/` live.

### 7 · Frontend konfigurieren

Öffne `index.html`, finde diese Zeile:
```js
const GITHUB_USER  = "DEIN_GITHUB_USERNAME";
```
Ersetze sie durch deinen tatsächlichen Username, committe und pushe.

### 8 · Personal Access Token erstellen

Du brauchst einen Token, damit dein Handy den Workflow auslösen darf.

1. https://github.com/settings/tokens → **Generate new token (classic)**
2. Name: `Daily Track`
3. Expiration: nach Wunsch (kein Ablauf möglich, aber unsicherer)
4. Scopes: nur **`repo`** ankreuzen
5. Generieren → kopieren

Beim ersten Speichern auf dem Handy fragt die Seite nach diesem Token. Er bleibt im Browser-LocalStorage (nur auf deinem Gerät).

### 9 · Home-Screen-Shortcut

**iOS (Safari):** Seite öffnen → Teilen-Symbol → „Zum Home-Bildschirm"

**Android (Chrome):** Drei-Punkte-Menü → „Zum Startbildschirm hinzufügen"

Fertig. Tippen, schieben, speichern.

---

## Ablauf täglich

1. Shortcut auf Handy tippen
2. 7 Slider bewegen (~30 Sek)
3. „In Notion speichern"
4. Workflow läuft ~15–30 Sek
5. Eintrag erscheint in Notion

## Fehlersuche

- **„401 Unauthorized"**: Token-Scope falsch oder abgelaufen
- **„404 Not Found"**: Username/Repo-Name in `index.html` stimmt nicht
- **Notion-Eintrag fehlt**: Workflow-Tab in GitHub anschauen → Logs
- **„property does not exist"**: Spaltenname in Notion ≠ erwartet (siehe Tabelle oben)

## Token zurücksetzen

Auf der Seite ganz nach unten scrollen → auf den Footer-Text tippen → bestätigen.
