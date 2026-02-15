# MCP Konfiguration - Detaillierte Anleitung

## 1. Git Konfiguration

Zunächst wird Git mit den persönlichen Daten konfiguriert:

```bash
git config --global user.name "josefpoettinger-lang"
git config --global user.email "244986495+josefpoettinger-lang@users.noreply.github.com"
```

Diese Befehle setzen den Benutzernamen und die E-Mail-Adresse global für alle Git-Operationen auf diesem System. Diese Daten werden in jedem Commit hinterlegt.

## 2. Repository klonen

```bash
git clone https://github.com/josefpoettinger-lang/surthraive.git
```

Dieses Kommando klont das `surthraive` Repository von GitHub in das lokale Verzeichnis.

## 3. Claude Desktop MCP-Server Konfiguration

Die MCP-Server werden in der Claude Desktop Config-Datei konfiguriert. Die Datei befindet sich unter:
```
$env:APPDATA\Claude\claude_desktop_config.json
```

Diese Datei kann mit folgendem Kommando geöffnet werden:
```powershell
notepad $env:APPDATA\Claude\claude_desktop_config.json
```

### Konfigurations-Struktur

Die Konfiguration definiert zwei MCP-Server:

#### a) Filesystem-Server
Dem Claude AI-Modell wird Zugriff auf das lokale Dateisystem gewährt. Der Server verbindet das angegebene Verzeichnis (`surthraive`) mit Claude Desktop.

#### b) GitHub-Server
Ermöglicht API-Zugriffe auf GitHub. Dazu wird ein Personal Access Token benötigt, das in der Umgebungsvariable `GITHUB_PERSONAL_ACCESS_TOKEN` hinterlegt wird.

### Konfigurationsdatei-Inhalt

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:/Users/Josef/Projekte/SW-Projekte/surthraive"
      ]
    },
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "{{GITHUB_TOKEN}}"
      }
    }
  }
}
```

**Hinweis:** Das Token `{{GITHUB_TOKEN}}` muss durch einen gültigen GitHub Personal Access Token ersetzt werden.
## 4. Konfigurationsdatei automatisiert schreiben (PowerShell)

Die Konfigurationsdatei kann auch automatisiert mit PowerShell erstellt werden:

### Methode 1: Mit Here-String
```powershell
$configContent = @'
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:/Users/Josef/Projekte/SW-Projekte/surthraive"
      ]
    },
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "{{GITHUB_TOKEN}}"
      }
    }
  }
}
'@

$configContent | Set-Content -Path "$env:APPDATA\Claude\claude_desktop_config.json" -Encoding UTF8
```

Dieser Befehl verwendet einen Here-String (`@'...'@`), um die JSON-Konfiguration zu definieren, und schreibt diese mit UTF-8-Kodierung in die Config-Datei.

### Methode 2: Mit direktem JSON-String
```powershell
$content = '{"mcpServers":{"filesystem":{"command":"npx","args":["-y","@modelcontextprotocol/server-filesystem","C:/Users/Josef/Projekte/SW-Projekte/surthraive"]},"github":{"command":"npx","args":["-y","@modelcontextprotocol/server-github"],"env":{"GITHUB_PERSONAL_ACCESS_TOKEN":"{{GITHUB_TOKEN}}"}}}}'

[System.IO.File]::WriteAllText("$env:APPDATA\Claude\claude_desktop_config.json", $content, (New-Object System.Text.UTF8Encoding $false))
```

Diese Alternative schreibt direkt einen JSON-String ohne Umwege.

## 5. Claude Desktop neuesten Versionen und Extensions

**Wichtig:** Mit Claude Desktop Version 1.1.2998+ hat sich das Konfigurationssystem geändert. Die MCP-Server werden nicht mehr ausschließlich über die Config-Datei geladen, sondern über ein Extensions-System in der Benutzeroberfläche.

### Native GitHub-Integration nutzen
Für GitHub-Zugriff wird empfohlen, die native GitHub-Integration zu verwenden:
- Klicke auf die GitHub-Option im Settings-Bereich
- Wähle "Verbinden" - dies authentifiziert über OAuth (kein Token-Fummelei nötig)

### Filesystem-Integration konfigurieren
- Öffne Settings in Claude Desktop
- Suche nach "filesystem" oder "Extensions"
- Klicke auf "Konfigurieren" und stelle sicher, dass es aktiviert ist

## 6. Mehrere Projekte zur Filesystem-Integration hinzufügen

Wenn mehrere Projekte via Filesystem-Server verfügbar sein sollen, können diese in den `args` hinzugefügt werden:

```bash
cd "C:\Users\Josef\Projekte\SW-Projekte"
git clone https://github.com/josefpoettinger-lang/dev-docs.git
```

Anschließend die Konfigurationsdatei aktualisieren:

```powershell
$config = Get-Content "$env:APPDATA\Claude\claude_desktop_config.json" -Raw | ConvertFrom-Json
$config.mcpServers.filesystem.args = @(
  "-y",
  "@modelcontextprotocol/server-filesystem",
  "C:/Users/Josef/Projekte/SW-Projekte/surthraive",
  "C:/Users/Josef/Projekte/SW-Projekte/dev-docs"
)
$json = $config | ConvertTo-Json -Depth 10
[System.IO.File]::WriteAllText("$env:APPDATA\Claude\claude_desktop_config.json", $json, (New-Object System.Text.UTF8Encoding $false))
```

Dieser Befehl:
1. Liest die aktuelle Konfigurationsdatei
2. Konvertiert sie von JSON in ein PowerShell-Objekt
3. Erweitert die `args`-Liste um das neue Projektverzeichnis
4. Schreibt die aktualisierte Konfiguration zurück

### Claude Desktop neu starten
Nach Konfigurationsänderungen muss Claude Desktop neu gestartet werden:

```powershell
Get-Process *claude* | Stop-Process -Force
```

Dieser Befehl beendet alle laufenden Claude-Prozesse erzwungen. Claude Desktop startet dann automatisch neu.

