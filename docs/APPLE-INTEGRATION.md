# Apple-Integration – Synchronisierung über Kurzbefehle

Tudilo zeigt Listen erst nach einer Synchronisierung. Der Button **Listen synchronisieren** startet den iOS-Kurzbefehl **Tudilo Notizen Sync**. Der Kurzbefehl muss in Apples Kurzbefehle-App einmalig angelegt werden.

## Ablauf
1. Kurzbefehl-Aktion **Notizen suchen**: Notizen finden, deren Titel `[todilo]` enthält.
2. Für jede gefundene Notiz Titel, Kategorie und Checklistenpunkte in eine JSON-Liste übertragen.
3. Die Daten per URL-kodiertem JSON an Tudilo zurückgeben, indem der Kurzbefehl diese Adresse mit dem Parameter `tudilo_sync` öffnet: `https://kistersi.github.io/ToDiLu/?tudilo_sync=<URL-kodiertes-JSON>`.

Beispiel für die Rückgabe:

```json
[{"id":"note-1","title":"[todilo] [privat] Wochenende","marker":"[todilo]","category":"privat","items":[{"id":"task-1","title":"Einkaufen","done":false}]}]
```

Tudilo filtert die Rückgabe selbst und übernimmt ausschließlich Einträge mit `marker: "[todilo]"` oder `[todilo]` im Titel. Nicht markierte Notizen werden ignoriert.

## Wichtige Grenze
Eine iOS-PWA kann Apple Notizen nicht selbstständig auslesen. Der Kurzbefehl ist zwingend erforderlich. Die PWA kann den Kurzbefehl starten und die strukturierte Rückgabe entgegennehmen; der eigentliche Kurzbefehl ist noch nicht automatisch auf dem iPhone installiert. Die Verfügbarkeit von Checklistenpunkten in Kurzbefehle muss auf dem Gerät geprüft werden. Bidirektionaler Abgleich und Konfliktauflösung sind noch nicht implementiert.


## Erkennung der Kurzbefehl-Einrichtung

iOS stellt einer PWA keine API bereit, mit der sie die installierten Kurzbefehle abfragen kann. Tudilo zeigt deshalb bis zu einer erfolgreichen Rückmeldung einen Einrichtungshinweis. Nach erfolgreicher Einrichtung muss der Kurzbefehl Tudilo mit `?tudilo_shortcut=ready` zurücköffnen. Dadurch wird die Einrichtung auf diesem Gerät als bestätigt gespeichert. Wird der Kurzbefehl später gelöscht, kann die PWA das nicht selbst erkennen; der Nutzer kann die Verbindung erneut testen bzw. zurücksetzen.
