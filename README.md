# FRITZ!Box Export Checksum Fixer

Ein leichtgewichtiges JavaScript-Tool (Node.js & Browser), um die Prüfsumme (CRC32) von FRITZ!Box-Konfigurationsdateien (`.export`) nach manuellen Änderungen zu korrigieren.

## 🛠 Was macht dieses Tool?
Wenn du eine FRITZ!Box-Sicherungsdatei manuell bearbeitest (z. B. um versteckte Einstellungen zu ändern), stimmt die am Ende der Datei gespeicherte Prüfsumme nicht mehr mit dem Inhalt überein. Die FRITZ!Box wird die Datei beim Wiederherstellen als "korrupt" ablehnen. 

Dieses Tool:
1. Scannt die Datei nach den relevanten Sektionen (`BINFILE`, `B64FILE`, `CFGFILE`).
2. Berechnet die CRC32-Prüfsumme nach dem spezifischen AVM-Verfahren neu.
3. Ersetzt die alte Checksumme am Ende der Datei (`**** END OF EXPORT ... ****`).

---

## 🚀 Nutzung

### 1. In Node.js
Du kannst das Skript ganz einfach in dein Node.js-Projekt einbinden.

```javascript
const fs = require("fs");
const FritzExportChecksum = require("./fritz-export-checksum.js");

function fixExportFile(inputPath, outputPath = inputPath + ".fixed") {
  const bytes = new Uint8Array(fs.readFileSync(inputPath));
  const checker = FritzExportChecksum.fromBytes(bytes);
  const result = checker.replaceChecksumAsBytes();

  fs.writeFileSync(outputPath, Buffer.from(result.updatedBytes));

  console.log("✅ Erfolg!");
  console.log("Alte Prüfsumme:", result.oldCrc);
  console.log("Neue Prüfsumme:", result.newCrc);
}

fixExportFile("./Einstellungen.export");
```

### 2. Im Browser
Nutze die beiliegende `index.html`, um Dateien direkt im Browser zu reparieren.

## 📂 Projektstruktur
* `fritz-export-checksum.js`: Die Kern-Logik als UMD-Modul.
* `index.html`: Grafische Oberfläche.

## ⚠️ Wichtiger Hinweis
Das Ändern von Konfigurationsdateien geschieht auf eigene Gefahr. Fehlerhafte Einstellungen können dazu führen, dass die FRITZ!Box nicht mehr korrekt startet. Erstelle immer eine Sicherheitskopie!