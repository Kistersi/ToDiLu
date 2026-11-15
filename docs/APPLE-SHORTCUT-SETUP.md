# Tudilo – Apple-Kurzbefehl einrichten

Diese Anleitung beschreibt den manuellen Aufbau des Apple-Kurzbefehls **Tudilo Notizen Sync**. Er liest Apple-Notizen mit dem Kennzeichen `[todilo]` aus und übergibt die gefundenen Listen an Tudilo.

> **Wichtig:** Diese Datei ist eine Aufbauanleitung, keine installierbare Kurzbefehle-Datei. Apple veröffentlicht einen installierbaren Kurzbefehl erst, wenn er in der Kurzbefehle-App erstellt und über iCloud geteilt wurde. Die konkrete Verarbeitung von Apple-Notiz-Checklisten muss auf dem Zielgerät getestet werden.

## Voraussetzungen

- iPhone oder iPad mit der Apple-App **Kurzbefehle**
- Tudilo ist in Safari erreichbar
- Die zu importierenden Apple-Notizen haben im Titel das Kennzeichen `[todilo]`

## 1. Notizen vorbereiten

Verwende das Kennzeichen im Titel der Notiz. Eine Kategorie kann optional in eckigen Klammern ergänzt werden:

- `[todilo] [privat] Wochenende`
- `[todilo] [arbeit] Teammeeting`
- `[todilo] [einkauf] Supermarkt`

Unterstützte Kategorien: `privat`, `arbeit`, `einkauf`, `wochenende`, `ideen`. Ohne Kategorie verwendet Tudilo `privat`.

## 2. Kurzbefehl anlegen

1. Öffne **Kurzbefehle** und tippe auf **+**.
2. Vergib exakt den Namen **Tudilo Notizen Sync**. Der Name ist wichtig, weil Tudilo diesen Kurzbefehl über die URL startet.
3. Füge die Aktion **Notizen suchen** hinzu.
4. Setze den Filter auf **Name enthält** `[todilo]`. Falls die Aktion eine Sortierung oder Begrenzung anbietet, stelle sicher, dass alle passenden Notizen berücksichtigt werden.

## 3. Daten aus den Notizen aufbereiten

Der Kurzbefehl muss die gefundenen Notizen in eine JSON-Liste umwandeln. Für jede Notiz wird ein Wörterbuch mit diesen Feldern benötigt:

| Feld | Bedeutung |
| --- | --- |
| `title` | Vollständiger Titel der Apple-Notiz |
| `marker` | Fester Wert `[todilo]` |
| `category` | Optional: eine Tudilo-Kategorie |
| `items` | Liste der Checklistenpunkte |

Jeder Eintrag in `items` enthält:

| Feld | Bedeutung |
| --- | --- |
| `title` | Text des Checklistenpunkts |
| `done` | `true`, wenn erledigt, sonst `false` |

Beispiel für die erwartete Rückgabe:

```json
[
  {
    "title": "[todilo] [privat] Wochenende",
    "marker": "[todilo]",
    "category": "privat",
    "items": [
      { "title": "Einkaufen", "done": false },
      { "title": "Paket abholen", "done": true }
    ]
  }
]
```

Baue die Daten in Kurzbefehle mit den Aktionen zum Wiederholen der gefundenen Notizen und zum Erstellen von Wörterbüchern auf. Die Bezeichnungen und Möglichkeiten der Aktionen können sich je nach iOS-Version unterscheiden. Entscheidend ist, dass am Ende ein gültiger JSON-Text im oben gezeigten Format entsteht.

**Hinweis zu Checklisten:** Apple-Notizen-Checklisten werden von Kurzbefehle nicht auf jeder iOS-Version gleich als strukturierte Aufgaben angeboten. Wenn die Punkte nicht einzeln auslesbar sind, ist die Synchronisierung der Aufgaben noch nicht möglich. Nicht einfach den gesamten Notiztext als einen erledigbaren Punkt importieren, ohne das ausdrücklich zu wollen.

## 4. JSON an Tudilo zurückgeben

1. Erzeuge aus dem Wörterbuch-Array einen JSON-Text.
2. Übergib diesen Text URL-kodiert im Parameter `tudilo_sync`.
3. Öffne die zusammengesetzte URL mit den Kurzbefehle-Aktionen **URL** und **URLs öffnen**.

Zieladresse:

```text
https://kistersi.github.io/ToDiLu/?tudilo_sync=<URL-kodiertes-JSON>
```

Ersetze den Platzhalter durch den tatsächlich URL-kodierten JSON-Text. Die spitzen Klammern gehören nicht in die fertige URL.

## 5. Einrichtung testen

1. Führe **Tudilo Notizen Sync** manuell aus.
2. Erlaube den Zugriff auf Apple-Notizen, falls iOS danach fragt.
3. Prüfe, ob Tudilo geöffnet wird und die markierten Notizen unter **Meine Listen** erscheinen.
4. Kontrolliere besonders Umlaute, Sonderzeichen, mehrere Notizen und erledigte Checklistenpunkte.

Tudilo akzeptiert nur Einträge mit `marker: "[todilo]"` oder dem Kennzeichen `[todilo]` im Titel. Andere Notizen werden ignoriert.

## 6. Kurzbefehl in Tudilo starten

Nach erfolgreicher Einrichtung kann der Button **Listen synchronisieren** den Kurzbefehl über dessen Namen starten. Die PWA kann nicht selbst feststellen, ob ein Kurzbefehl installiert ist oder später gelöscht wurde.

## Grenzen des aktuellen Standes

- Eine iOS-PWA kann Apple-Notizen nicht direkt auslesen.
- Die Installation eines Kurzbefehls erfordert eine Bestätigung des Nutzers in iOS.
- Ein iCloud-Freigabelink kann erst ergänzt werden, wenn der Kurzbefehl in Apple Kurzbefehle erstellt und dort geteilt wurde.
- Der aktuelle Import ersetzt die in Tudilo gespeicherten synchronisierten Listen und Aufgaben durch die empfangenen Daten.
- Bidirektionale Synchronisierung, Konfliktauflösung und automatischer Abgleich sind noch nicht umgesetzt.
- Die URL-Übergabe ist für kleine bis mittlere Datenmengen gedacht. Bei sehr großen Notizsammlungen kann die URL zu lang werden.

---

**Projekt:** [Kistersi/ToDiLu](https://github.com/Kistersi/ToDiLu)  
**Tudilo-Ziel:** [App öffnen](https://kistersi.github.io/ToDiLu/)
