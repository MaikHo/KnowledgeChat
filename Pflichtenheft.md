
# Pflichtenheft – Projekt "MaikHo Knowledge Chat"

## 1. Zielsetzung
Das Ziel des Projekts ist die Entwicklung einer lokalen, plattformunabhängigen Desktop-Anwendung, die:
1. Eigene Wissensdokumente (Markdown, PDF, ggf. weitere Formate) automatisiert einliest, indexiert und semantisch durchsuchen kann.
2. Einen lokalen LLM-Server (z. B. LM Studio) als Antwortgenerator nutzt, ergänzt durch RAG (Retrieval-Augmented Generation).
3. Über eine Avalonia-UI verfügt, die Markdown mit Syntax-Highlighting darstellen kann.
4. Ein Gedächtnis besitzt, das sowohl markierte Inhalte in SQLite als auch komplette Chats in JSON speichert.
5. Erweitert werden kann, um Websuche und Shell-Befehle (Dateioperationen) auszuführen.

## 2. Rahmenbedingungen
1. **Lokal, offline**: Alle Daten und Verarbeitungen laufen ausschließlich auf dem lokalen Rechner.
2. **Betriebssysteme**: Linux (Primär), Windows (Sekundär).
3. **Technologien**:
   - C# (.NET 8+)
   - AvaloniaUI 11 + ReactiveUI
   - Microsoft Semantic Kernel
   - ChromaDB (lokal)
   - System.Text.Json (Serialisierung)
   - SQLite (Archiv markierter Inhalte)
4. **Bedienkonzept**: Maus + Tastatur, Chat-ähnliche Bedienung.

## 3. Funktionsumfang

### 3.1 Datenaufnahme
- Import von `.md`-Dateien (Markdown → Text via Markdig oder Markdown.Avalonia).
- Import von `.pdf`-Dateien (Text-Extraktion via UglyToad.PdfPig).
- Optional: Import weiterer Formate (TXT, CSV).
- Automatischer Ordner-Scan bei Start oder auf Knopfdruck.

### 3.2 Verarbeitung
- Erzeugung von Embeddings (lokal, kompatibel mit LM Studio oder sentence-transformers).
- Speicherung der Vektoren in ChromaDB.
- Suche nach relevanten Textstücken bei Benutzeranfragen.

### 3.3 Chat-Funktion
- Eingabe von Fragen im Chatfenster.
- Anzeige von Antworten als Markdown mit Syntax-Highlighting.
- Unterstützung für Codeblöcke (```sprache).
- Kontextanzeige: Die Antwort enthält die Quellen (Dateiname, Pfad).

### 3.4 Gedächtnis & Archiv
- Markierte Passagen im Chat oder in Dokument-Viewer → Speicherung in SQLite mit Zeitstempel.
- Vollständige Chats als JSON-Dateien im Archivordner speichern.
- Suchfunktion im Archiv (nach Schlagwort, Datum, Quelle).

### 3.5 Erweiterungen
- Websuche via API (z. B. Bing, Google Custom Search).
- Shell-Kommandos:
  - Datei erstellen
  - Datei öffnen
  - Verzeichnis anzeigen
  - Datum/Uhrzeit abfragen
  - Weitere einfache Systembefehle

## 4. Nicht-funktionale Anforderungen
- **Performance**: Antwortzeiten < 2 Sek. bei Chroma-Suche, < 5 Sek. gesamt.
- **UI**:
  - Dark/Light Theme umschaltbar
  - Responsive Layout
  - Syntax-Highlighting (mind. C#, JSON, XML, Bash)
- **Sicherheit**:
  - Keine Datenübertragung ins Internet ohne Benutzerfreigabe
  - Lokale Dateizugriffe nur in freigegebenen Ordnern

## 5. Architekturübersicht

```
[ Avalonia UI (ReactiveUI) ]
     ├── Markdown Renderer (Markdown.Avalonia)
     ├── Chat-Eingabe + Verlauf
     └── Archivverwaltung (SQLite + JSON)
           |
[ Semantic Kernel ]
     ├── Embedding Service
     ├── RAG Controller
     └── LM Studio API Connector
           |
[ ChromaDB lokal (Docker/Python) ]
           |
[ Dateiscanner (.md, .pdf) ]
```

## 6. Abnahmekriterien
Das Projekt gilt als erfolgreich abgeschlossen, wenn:
1. Mindestens 100 eigene Markdown/PDF-Dateien importiert, indexiert und durchsucht werden können.
2. Eine Frage zu einem gespeicherten Inhalt mit Quellenangabe beantwortet wird.
3. Markierte Inhalte in SQLite gespeichert und wieder auffindbar sind.
4. Ein kompletter Chat als JSON archiviert und geladen werden kann.
5. Syntax-Highlighting für C#, JSON und Bash im UI funktioniert.
6. Shell-Befehle im kontrollierten Modus ausgeführt werden können.
