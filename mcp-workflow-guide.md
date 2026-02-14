# MCP Workflow Guide – Claude Desktop Chat + Repository

## Grundprinzip

Du beschreibst, was du brauchst. Claude führt es aus. Du genehmigst jeden Zugriff.

Zwei Werkzeuge arbeiten zusammen:

- **Filesystem** → lokale Dateien und Ordner (lesen, erstellen, bearbeiten, löschen, suchen)
- **GitHub** → Versionsverwaltung (commit, push, pull, branches, issues)

---

## Was du im Chat sagen kannst

### Dateien & Ordner verwalten

| Was du sagst | Was Claude tut |
|---|---|
| "Zeig mir die Projektstruktur" | Listet alle Ordner und Dateien auf |
| "Erstelle einen Ordner `src/components`" | Legt den Ordner an |
| "Erstelle eine Datei `README.md` mit einer Projektbeschreibung" | Erstellt die Datei mit Inhalt |
| "Öffne die Datei `config.js`" | Zeigt den Dateiinhalt |
| "Aendere in `index.html` den Titel auf SurThraive" | Bearbeitet die Datei gezielt |
| "Suche nach allen Dateien die API_KEY enthalten" | Durchsucht das Projekt |

### Versionsverwaltung (Git/GitHub)

| Was du sagst | Was Claude tut |
|---|---|
| "Committe alle Aenderungen mit der Nachricht Feature X hinzugefuegt" | Staged, committed und pushed |
| "Zeig mir die letzten 5 Commits" | Listet die Commit-Historie |
| "Erstelle einen Branch feature/login" | Erstellt den Branch |
| "Welche Dateien haben sich seit dem letzten Commit geaendert?" | Zeigt den Git-Status |
| "Erstelle ein Issue: Bug im Login-Formular" | Legt ein GitHub Issue an |
| "Pull die neuesten Aenderungen vom Desktop" | Holt Aenderungen vom Remote |

---

## Empfohlener Arbeitsablauf

### Vor dem Arbeiten (Synchronisation)

Sage: "Pull die neuesten Aenderungen von GitHub"

Damit stellst du sicher, dass dein Laptop den aktuellen Stand vom Desktop hat.

### Waehrend dem Arbeiten

Beschreibe einfach, was du brauchst. Beispiele:

- "Erstelle eine neue Datei docs/api-spec.md mit einer API-Spezifikation fuer den Login-Endpoint"
- "Refactore die Funktion calculateTotal in utils.js - sie soll auch Rabatte beruecksichtigen"
- "Fuege einen neuen Ordner tests hinzu und erstelle darin eine Testdatei fuer die User-Klasse"

### Nach dem Arbeiten (Sichern)

Sage: "Committe und pushe alle Aenderungen mit einer passenden Commit-Nachricht"

### Am anderen Rechner (Desktop)

Dort einfach git pull ausfuehren oder in Claude Code/Cowork sagen: "Pull die neuesten Aenderungen"

---

## Berechtigungen

Jede Aktion erfordert deine Genehmigung:

- **Schreibgeschuetzte Tools** (lesen, suchen, auflisten) - Du genehmigst beim ersten Mal, danach geht es automatisch
- **Schreib-/Loesch-Tools** (erstellen, bearbeiten, loeschen) - Genehmigung bei jeder Aktion empfohlen

Die Einstellung "Genehmigung erforderlich" ist der sicherste Modus.

---

## Chat vs. Cowork - wann was verwenden?

| Aufgabe | Empfehlung |
|---|---|
| Schnelle Dateiaenderungen | Chat |
| Fragen zum Code / Reviews | Chat |
| Recherche und Diskussion | Chat |
| Groessere Refactorings (viele Dateien) | Cowork |
| Neue Features mit mehreren Schritten | Cowork |
| Dokumente generieren (Word, PDF) | Cowork |
| Lange autonome Aufgaben | Cowork |

---

## Synchronisation Desktop und Laptop

Desktop PC --> git push --> GitHub --> git pull --> Laptop
Laptop --> git push --> GitHub --> git pull --> Desktop PC

**Goldene Regel:** Immer erst pullen, bevor du arbeitest. Immer pushen, wenn du fertig bist. Dann bleiben beide Rechner synchron.

---

## Tipps

- Du musst keine Git-Befehle kennen - sag einfach in natuerlicher Sprache, was passieren soll
- Bei Konflikten (gleiche Datei auf beiden Rechnern geaendert) hilft Claude dir beim Loesen
- Nutze aussagekraeftige Commit-Nachrichten - das hilft beim Nachvollziehen der Aenderungen
- Der Chat verbraucht weniger Tokens als Cowork - nutze ihn fuer alles, was keine lange autonome Arbeit erfordert
